# Use node image to build Vue app
FROM node:latest AS build

RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app

WORKDIR /home/node/app

COPY --chown=node:node package*.json ./

USER node

RUN npm install

COPY --chown=node:node . .

RUN npm run build

# Copy build to nginx image and start
FROM nginx:latest

ARG PORT=${CLIENT_PORT}

COPY --from=build /home/node/app/dist /usr/share/nginx/html

RUN rm /etc/nginx/conf.d/default.conf

COPY deploy/nginx.conf /etc/nginx/conf.d

EXPOSE ${PORT}

CMD ["nginx", "-g", "daemon off;"]