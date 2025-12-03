FROM node:20.19.5-alpine3.22 AS build
WORKDIR /opt/server
COPY *.js .
COPY package.json .
# this may add extra cache memory
RUN npm install


FROM node:20.19.5-alpine3.22
# create roboshop group user
RUN addgroup -S roboshop && adduser -S -G roboshop roboshop
WORKDIR /opt/server
COPY --from=build /opt/server /opt/server
ENV MONGO="true" \
    MONGO_URL="mongodb://mongodb:27017/catalogue"
RUN chown -R roboshop:roboshop /opt/server
USER roboshop
CMD ["node", "server.js"]