FROM node:20.19.5-alpine3.22 AS build
WORKDIR /opt/server
COPY *.js .
COPY package.json .
# this may add extra cache memory
RUN npm install


FROM node:20.19.5-alpine3.22
# create roboshop group user
WORKDIR /opt/server
RUN addgroup -S roboshop && adduser -S -G roboshop roboshop && \
    chown -R roboshop:roboshop /opt/server
EXPOSE 8080
LABEL com.project="roboshop" \
      component="catalogue" \
      created_by="Vijay Amudalapalli"
ENV MONGO="true" \
    MONGO_URL="mongodb://mongodb:27017/catalogue"
COPY --from=build --chown=roboshop:roboshop /opt/server /opt/server
USER roboshop
CMD ["server.js"]
ENTRYPOINT ["node"]