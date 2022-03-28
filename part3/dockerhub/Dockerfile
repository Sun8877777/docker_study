FROM node:14-slim as build
RUN mkdir -p /var/node/
ADD src/ /var/node/
WORKDIR /var/node/
RUN npm install

FROM node:14-alpine
ARG VERSION=V1.1
LABEL org.label-schema.version=$VERSION
ENV NODE_ENV="production"
COPY --from=build /var/node /var/node
WORKDIR /var/node
EXPOSE 3000
ENTRYPOINT ["./bin/www"]
