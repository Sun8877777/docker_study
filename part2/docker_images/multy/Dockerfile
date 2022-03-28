FROM node as build
ARG SRC_DIR=/var/node
RUN mkdir -p $SRC_DIR
ADD src/ $SRC_DIR
WORKDIR $SRC_DIR
RUN npm install

FROM node:alpine
ARG VERSION=V1.1
LABEL org.label-schema.version=$VERSION
ENV NODE_ENV="production"
ENV PORT 3000
COPY --from=build /var/node /var/node
WORKDIR /var/node
EXPOSE ${PORT}
ENTRYPOINT ["./bin/www"]