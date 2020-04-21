FROM registry.access.redhat.com/ubi8/nodejs-12 as base

FROM base as builder

WORKDIR /opt/app-root/src

COPY package*.json ./

RUN npm ci

COPY public public 
COPY src src 

RUN npm run build

FROM base

COPY --from=builder  /opt/app-root/src/build build

COPY package*.json ./

RUN npm ci --only=production

COPY --chown=1001:0 server server
RUN chmod -R g=u server

ENV PORT=8080

LABEL com.example.source="https://github.com/acme/nodejsapp"
LABEL com.example.version="1.0"

ARG ENV=production
ENV NODE_ENV $ENV
CMD npm run $NODE_ENV