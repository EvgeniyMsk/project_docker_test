FROM node:18-alpine as development
WORKDIR /usr/src/app
COPY --chown=node:node package*.json ./
RUN npm ci
COPY --chown=node:node . .

USER node
FROM node:18-alpine as build
WORKDIR /usr/src/app
COPY --chown=node:node package*.json ./
COPY --chown=node:node --from=development /usr/src/app/node_modules ./node_modules
COPY --chown=node:node . .
RUN npm run build
RUN npm ci --only=production && npm cache clean --force

USER node
FROM nginx as production
COPY --chown=node:node nginx.conf /etc/nginx/nginx.conf
COPY --from=build /usr/src/app/dist/frontend/browser /etc/nginx/html