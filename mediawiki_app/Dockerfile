FROM centos:7

RUN yum install -y --setopt=tsflags=nodocs centos-release-scl

RUN yum install -y wget httpd24-httpd rh-php73 rh-php73-php rh-php73-php-mbstring rh-php73-php-mysqlnd rh-php73-php-gd rh-php73-php-xml && yum clean all

WORKDIR /opt/rh/httpd24/root/var/www/html

RUN wget https://releases.wikimedia.org/mediawiki/1.35/mediawiki-1.35.9.tar.gz && \
    tar -zxf mediawiki-1.35.9.tar.gz && \
    mv mediawiki-1.35.9 mediawiki && \
    rm -f mediawiki-1.35.9.tar.gz

RUN sed -i -e 's|^DocumentRoot "/opt/rh/httpd24/root/var/www/html"|DocumentRoot "/opt/rh/httpd24/root/var/www/html/mediawiki"|' -e 's|    DirectoryIndex index.html|    DirectoryIndex index.html index.html.var index.php|' -e 's|^<Directory "/opt/rh/httpd24/root/var/www/html">|<Directory "/opt/rh/httpd24/root/var/www/html/mediawiki">|' /opt/rh/httpd24/root/etc/httpd/conf/httpd.conf

RUN chown -R apache:apache /opt/rh/httpd24/root/var/www/html/mediawiki

RUN chmod 755 /opt/rh/httpd24/root/var/www/html/mediawiki/

CMD /opt/rh/httpd24/root/usr/sbin/apachectl -DFOREGROUND