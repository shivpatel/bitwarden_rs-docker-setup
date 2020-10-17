FROM ubuntu:18.04

RUN apt update
RUN apt install nginx -y

RUN apt-get install software-properties-common -y && \
    add-apt-repository universe && \
    add-apt-repository ppa:certbot/certbot && \
    apt-get update && \
    apt-get install certbot python-certbot-nginx -y

RUN nginx -v && certbot --version

CMD ["nginx", "-g", "daemon off;"]
