FROM node:8

WORKDIR /usr/app

COPY ./package.json /usr/app

RUN npm install

ADD . /usr/app

# EXPOSE 8090

CMD [ "npm", "start" ]
