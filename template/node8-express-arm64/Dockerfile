FROM arm64v8/alpine:3.7

RUN apk add --no-cache nodejs nodejs-npm ca-certificates
RUN apk --no-cache add curl \
    && echo "Pulling watchdog binary from Github." \
    && curl -sSL https://github.com/openfaas-incubator/of-watchdog/releases/download/0.2.3/of-watchdog-arm64 > /usr/bin/fwatchdog \
    && chmod +x /usr/bin/fwatchdog \
    && apk del curl --no-cache

WORKDIR /root/

COPY package.json   .

RUN npm i
COPY index.js       .
COPY function       function
WORKDIR /root/function

ENV NPM_CONFIG_LOGLEVEL warn
RUN npm i || :
WORKDIR /root/

ENV cgi_headers="true"

ENV fprocess="node index.js"

HEALTHCHECK --interval=1s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]