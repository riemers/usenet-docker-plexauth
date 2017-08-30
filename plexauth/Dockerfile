FROM nginx:1.13-alpine
MAINTAINER riemers
ARG MYSQL_PLEXAUTH_SQL
ARG G_CAPTCHA

ENV MYSQL_PLEXAUTH_SQL "$MYSQL_PLEXAUTH_SQL"
ENV G_CAPTCHA "$G_CAPTCHA"

# Install PHP7 and GeoIP Packages for Organizr
RUN \
 apk add --no-cache \
	php7 \
	php7-curl \
	php7-fpm \
        php7-session \
        php7-json \
        php7-openssl \
        php7-pdo_mysql \
	geoip-dev \
        supervisor \
	bash
        
# Build nginx with custom modules (geoip/realip/http_sub/headers_more)
RUN apk \ 
        --update add git && \
    mkdir -p /tmp/src && \
    cd /tmp/src && \
    git clone https://github.com/hjone72/PlexAuth && \
    git clone https://github.com/hjone72/PlexAuth_Pages && \
    mv ./PlexAuth /usr/share/nginx/html/plexauth && \
    cp -rf PlexAuth_Pages/PlexAuth/* /usr/share/nginx/html/plexauth/ && \
    sed "s/connectSQL.*/${MYSQL_PLEXAUTH_SQL}/g" < PlexAuth_Pages/PlexAuth/inc/pages/invite.page.php > /usr/share/nginx/html/plexauth/inc/pages/invite.page.php && \
    sed "s/connectSQL.*/${MYSQL_PLEXAUTH_SQL}/g" < PlexAuth_Pages/PlexAuth/inc/pages/join.page.php > /tmp/src/join.page.php && \
    sed "s/6LdEAw4UAABBAPCLJjavlf0w4DkHFXf2w2q816zr/${G_CAPTCHA}/g" < /tmp/src/join.page.php > /usr/share/nginx/html/plexauth/inc/pages/join.page.php && \
    rm -rf PlexAuth_Pages && \
    apk del git && \
    rm -rf /tmp/src && \
    rm -rf /var/cache/apk/*
    
# add local files
COPY root/ /

# fix permissions
RUN chown nginx -R /usr/share/nginx/html/plexauth && \
    chown nginx /var/lib/php7/sessions && \
    chmod 755 -R /usr/share/nginx/html/plexauth

# Change default nginx to also startup fpm in our case.
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
