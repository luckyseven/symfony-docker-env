FROM debian:jessie

RUN apt-get update && apt-get install -y \
    nginx

# Add config files
ADD nginx.conf /etc/nginx/
ADD main.conf /etc/nginx/sites-available/
ADD upstream.conf /etc/nginx/conf.d/

RUN ln -s /etc/nginx/sites-available/main.conf /etc/nginx/sites-enabled/main
RUN rm /etc/nginx/sites-enabled/default

RUN usermod -u 1000 www-data

CMD ["nginx"]

EXPOSE 80
EXPOSE 443
