FROM nginx:1.13-alpine
MAINTAINER riemers

# Install PHP7 and GeoIP Packages for Organizr
RUN \
 apk add --no-cache \
	php7 \
	php7-curl \
	php7-fpm \
        php7-session \
	geoip-dev \
        supervisor \
	bash
        
# Build nginx with custom modules (geoip/realip/http_sub/headers_more)
RUN apk \ 
        --update add git && \
    mkdir -p /tmp/src && \
    cd /tmp/src && \
    git clone -b session_regenerate https://github.com/hjone72/PlexAuth && \
    mv ./PlexAuth /usr/share/nginx/html/plexauth && \
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
