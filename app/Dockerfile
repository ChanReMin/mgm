FROM node:16-alpine AS base
WORKDIR /home/app
EXPOSE 8000

COPY package*.json /home/app 

RUN npm install 

COPY . /home/app

ENTRYPOINT [ "node", "app.js" ]