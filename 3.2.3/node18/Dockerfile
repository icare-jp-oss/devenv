FROM node:18.19.0-bullseye AS node-18

FROM ruby:3.2.3-bullseye

WORKDIR /app

COPY --from=node-18 /usr/local/bin/node /usr/local/bin/
COPY --from=node-18 /usr/local/lib/node_modules/ /usr/local/lib/node_modules/
COPY --from=node-18 /opt /opt/

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
