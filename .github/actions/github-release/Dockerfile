FROM node:26.13-slim@sha256:18f03e4b532a63baf09809fe8349c2171807f1818cb6c5230fb12aecd5beda65

COPY . /action
WORKDIR /action

RUN npm install --production

ENTRYPOINT ["node", "/action/main.js"]
