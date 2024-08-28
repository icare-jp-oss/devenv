# syntax=docker/dockerfile:1
FROM node:18.20.2-bullseye AS node

FROM ruby:3.2.3-bullseye

# Let NodeModules install into /pnpm.
ENV PNPM_HOME=/pnpm

# Let RubyGems install gems into /bundle and install executables into /bundle/bin.
ENV GEM_HOME=/bundle

# Let Bundler install gems into /bundle.
ENV BUNDLE_PATH=/bundle

# Let Bundler install executables into /bundle/bin.
ENV BUNDLE_BIN=${BUNDLE_PATH}/bin

# Let shells search executables from /bundle/bin.
ENV PATH="${BUNDLE_BIN}:${PATH}"

WORKDIR /app

COPY --from=node /usr/local/bin/node /usr/local/bin/
COPY --from=node /usr/local/lib/node_modules/ /usr/local/lib/node_modules/
COPY --from=node /opt /opt/

RUN <<EOF
  ln -s /usr/local/bin/node /usr/local/bin/nodejs
  ln -s /usr/local/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm
  ln -s /usr/local/lib/node_modules/npm/bin/npx-cli.js /usr/local/bin/npx
  ln -s /usr/local/lib/node_modules/corepack/dist/corepack.js /usr/local/bin/corepack
  yarn_version=$(ls /opt | grep yarn-v | cut -d "-" -f 2)
  ln -s /opt/yarn-${yarn_version}/bin/yarn /usr/local/bin/yarn
  ln -s /opt/yarn-${yarn_version}/bin/yarnpkg /usr/local/bin/yarnpkg
  # smoke tests
  node --version
  npm --version
  yarn --version
  corepack --version
EOF
