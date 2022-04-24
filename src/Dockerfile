FROM node:current-alpine3.15 AS base
WORKDIR /app
COPY package*.json ./

FROM base AS dependencies
RUN npm set progress=false && npm config set depth 0
RUN npm install
RUN cp -R node_modules prod_node_modules

FROM base AS release
COPY --from=dependencies /app/prod_node_modules ./node_modules
COPY . .
EXPOSE 8080
CMD ["node", "server.js"]