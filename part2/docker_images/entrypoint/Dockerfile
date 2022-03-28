FROM node:14-slim
LABEL org.label-schema.version=v1.1
ARG SRC_DIR=/var/node
ENV NODE_ENV="production"
ENV PORT 3001

RUN mkdir -p $SRC_DIR
ADD src/ $SRC_DIR
WORKDIR $SRC_DIR
RUN npm install
EXPOSE ${PORT}
ENTRYPOINT ./bin/www