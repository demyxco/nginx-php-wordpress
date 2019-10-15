FROM demyx/nginx-php-wordpress

LABEL sh.demyx.image demyx/nginx-php-wordpress:bedrock
LABEL sh.demyx.maintainer Demyx <info@demyx.sh>
LABEL sh.demyx.url https://demyx.sh
LABEL sh.demyx.github https://github.com/demyxco
LABEL sh.demyx.registry https://hub.docker.com/u/demyx

# Packages
RUN set -ex; \
    apk add --update --no-cache \
    composer \
    git

# Bedrock
RUN set -ex; \
	su -c 'composer create-project roots/bedrock /tmp/bedrock; composer clearcache' -s /bin/sh www-data; \
    tar -czf /usr/src/bedrock.tgz -C /tmp/bedrock .; \
    rm -rf /usr/src/wordpress; \
    rm -rf /tmp/*; \
    \
    echo -e '# Prevent PHP scripts from being executed inside the uploads folder.\nlocation ~* /app/uploads/.*.php\$ {\n    deny all;\n}\nlocation ~* .(blade.php)$ {\n     deny all;\n}' > /etc/nginx/common/bedrock.conf; \
    sed -i "s|/var/www/html|/var/www/html/web|g" /etc/nginx/nginx.conf

COPY 00-init /etc/cont-init.d/00-init

EXPOSE 80 9000

WORKDIR /var/www/html

ENTRYPOINT ["/init"]
