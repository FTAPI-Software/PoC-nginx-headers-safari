FROM ubuntu:latest
RUN apt update -y && \
    apt install -y nginx libnginx-mod-http-headers-more-filter
ADD nginx.conf /etc/nginx/nginx.conf
ADD public.crt /etc/nginx/public.crt
ADD private.rsa /etc/nginx/private.rsa

CMD ["nginx", "-g", "daemon off;"]
