FROM nginx:1.25.3-alpine3.18

RUN apk --update add tzdata && \
    cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
    apk del tzdata && \
    rm -rf /var/cache/apk/*

# install packages
RUN apk update && \
    apk upgrade && \
    apk add --update --no-cache \
    bash \
    nginx \
    sudo \
    tzdata \
    vim \
    wget \
    tar \
    curl

RUN mkdir -p /var/www/dev-k8s.menta.me
RUN chown -R root:root /var/www/dev-k8s.menta.me
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./dev-k8s.menta.me.conf /etc/nginx/conf.d/dev-k8s.menta.me.conf

EXPOSE 80
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]