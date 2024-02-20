FROM node:16-bullseye AS node-16

FROM icarejposs/ruby:3.3.0-bullseye-patched

ENV LANG C.UTF-8

RUN apt-get update -qq \
    && apt-get install -y \
      apt-utils \
      build-essential \
      chromium \
      unzip \
      vim \
      wget \
    && rm -rf /var/lib/apt/lists/*


FROM base AS rails-runner

WORKDIR /app

COPY --from=node-16 /usr/local/bin/node /usr/local/bin/
COPY --from=node-16 /usr/local/lib/node_modules/ /usr/local/lib/node_modules/
COPY --from=node-16 /opt /opt/

RUN ln -s /usr/local/bin/node /usr/local/bin/nodejs && \
    ln -s /usr/local/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm && \
    ln -s /usr/local/lib/node_modules/npm/bin/npx-cli.js /usr/local/bin/npx && \
    yarn_version=$(ls /opt | grep yarn-v | cut -d "-" -f 2) && \
    ln -s /opt/yarn-${yarn_version}/bin/yarn /usr/local/bin/yarn && \
    ln -s /opt/yarn-${yarn_version}/bin/yarnpkg /usr/local/bin/yarnpkg && \
    # smoke tests
    node --version && \
    npm --version && \
    yarn --version

RUN apt-get update -qq \
    && apt-get install -y \
      ca-certificates \
      chromium-driver \
      fonts-liberation \
      fonts-ipafont \
      gosu \
      libappindicator1 \
      libgbm-dev \
      libpq-dev \
      libpython3-dev \
      unzip \
      postgresql-client \
      python3-pip \
      wget \
      xvfb \
    && rm -rf /var/lib/apt/lists/* \
    && bundle config set force_ruby_platform true

RUN wget https://noto-website-2.storage.googleapis.com/pkgs/NotoSansCJKjp-hinted.zip \
  && mkdir /usr/share/fonts/noto \
  && unzip NotoSansCJKjp-hinted.zip NotoSansCJKjp-Regular.otf NotoSansCJKjp-Bold.otf -d /usr/share/fonts/noto/ \
  && fc-cache -v

RUN pip3 install awscli --upgrade --user
RUN echo "export PATH=~/.local/bin:$PATH" >> ~/.bashrc
