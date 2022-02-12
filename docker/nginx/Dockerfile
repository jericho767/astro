FROM nginx
RUN apt-get update
RUN apt-get install software-properties-common -y
RUN apt-get install gnupg -y
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 8C47BE8E75BCA694
RUN add-apt-repository ppa:certbot/certbot -y
RUN apt-get install certbot python-certbot-nginx -y
ARG ENVIRONMENT
ARG API_DOMAIN
ARG APP_DOMAIN
ADD ./docker/nginx/scripts/configure-$ENVIRONMENT.sh /configure.sh
# Update Nginx server block with correct domains for API and APP
ADD ./docker/nginx/$ENVIRONMENT.conf /etc/nginx/conf.d/default.conf
RUN ["chmod", "+x", "/configure.sh"]
RUN /configure.sh
