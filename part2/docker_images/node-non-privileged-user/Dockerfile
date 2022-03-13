FROM node:14-slim
LABEL org.label-schema.version=v1.1
RUN useradd -ms /bin/bash node_user
USER node_user

ADD src/ /home/node_user
WORKDIR /home/node_user
RUN npm install
EXPOSE 3000
CMD ./bin/www