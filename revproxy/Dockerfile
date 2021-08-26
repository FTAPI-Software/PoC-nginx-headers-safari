FROM ubuntu:latest
RUN apt update -y && \
    apt install -y nginx libnginx-mod-http-headers-more-filter
ADD nginx.conf /etc/nginx.conf

CMD ["nginx", "-g", "daemon off;"]
