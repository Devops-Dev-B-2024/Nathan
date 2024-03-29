FROM node:20-alpine

WORKDIR ./api

COPY ./YPathe .

RUN rm -rf ./node_modules

RUN npm install

CMD ["npm", "start"]

EXPOSE 3000 