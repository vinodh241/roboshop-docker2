FROM node:20
WORKDIR /opt/server
COPY package.json .
COPY *.js .
RUN npm install 

ENV MONGO="true" \
    REDIS_URL="rdis://redis" \
    MONGO_URL="mongodb://mongodb:27017/users"

CMD [ "node","server.js"]
