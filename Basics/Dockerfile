FROM alpine:3.9
LABEL "Mail":"arvon2014@gmail.com"\
      "version":"v1.2.6"
ENV RUN_USER daemon 
ENV RUN_GROUP daemon
ENV WSL_INSTALL_DIR=/opt/navi
ENV DB_HOST 127.0.0.1
ENV DB_PORT 3306
ENV DB_DATABASE homestead
ENV DB_USERNAME homestead
ENV DB_PASSWORD secret
ENV LOGIN_COPTCHA false 

ARG WEBSTACK_VERSION=v1.2.6
ARG DOWNLOAD_URL=https://github.com/hui-ho/WebStack-Laravel/archive/${WEBSTACK_VERSION}.tar.gz
EXPOSE 8000

COPY entrypoint.sh /entrypoint.sh
RUN apk update -qq \
	&& apk upgrade \
	&& apk add --no-cache tini \
	   curl composer \
       php-pdo php-fileinfo  php-tokenizer php-gd php-dom  php-xmlwriter php-xml php-pdo_mysql php-session php-simplexml \
    && rm -rf /var/cache/apk/*
    
RUN mkdir -p ${WSL_INSTALL_DIR}
RUN curl -L  ${DOWNLOAD_URL} | tar -xz --strip-components=1 -C "${WSL_INSTALL_DIR}" 
RUN echo install dir ${WSL_INSTALL_DIR}
RUN ls ${WSL_INSTALL_DIR}
RUN cd ${WSL_INSTALL_DIR} && pwd && ls && composer install && cp .env.example .env 
RUN chown -R ${RUN_USER}:${RUN_GROUP} ${WSL_INSTALL_DIR}
RUN chown -R ${RUN_USER}:${RUN_GROUP} /entrypoint.sh

WORKDIR ${WSL_INSTALL_DIR}
CMD ["/entrypoint.sh", "serve"]
ENTRYPOINT ["/sbin/tini", "--"]
