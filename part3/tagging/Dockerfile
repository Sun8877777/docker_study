FROM node
LABEL org.label-schema.version=v1.1
ENV NODE_ENV="development"
ENV PORT 3000
RUN mkdir -p /var/node
ADD src/ /var/node
WORKDIR /var/node
RUN npm install
EXPOSE $PORT
CMD ./bin/www