FROM ubuntu:trusty

RUN apt-get update
RUN apt-get -y install python=2.7.5-5ubuntu3 python-pip=1.5.4-1ubuntu4
RUN apt-get install -y wget=1.15-1ubuntu1.14.04.5

#get Ubuntu trusty version of OpenSSL & libssl from the Debian snapshots
RUN wget http://snapshot.debian.org/archive/debian/20140106T220635Z/pool/main/o/openssl/openssl_1.0.1f-1_amd64.deb -O /tmp/openssl_1.0.1f-1_amd64.deb
RUN wget http://snapshot.debian.org/archive/debian/20140106T220635Z/pool/main/o/openssl/libssl1.0.0_1.0.1f-1_amd64.deb -O /tmp/libssl1.0.0_1.0.1f-1_amd64.deb

#install these versions of OpenSSL
RUN dpkg -i /tmp/libssl1.0.0_1.0.1f-1_amd64.deb
RUN dpkg -i /tmp/openssl_1.0.1f-1_amd64.deb

#adds the snakeoil self signed certificate
RUN apt-get -y install ssl-cert=1.0.33

#install apache and wsgi to host the Flask app
RUN apt-get -y install apache2=2.4.7-1ubuntu4.22 libapache2-mod-wsgi=3.4-4ubuntu2.1.14.04.2

#Simple Flask based webapp
RUN mkdir -p /srv/www/app

#copy files over
ADD app /srv/www/app

#install Flask and related modules
RUN pip install Flask==1.0.3 Flask-Login==0.4.1 Flask-WTF==0.14.2

#copy conf files for our new app over to Apache's conf
ADD conf /etc/apache2/sites-available

#enable various Apache modules
RUN a2enmod wsgi
RUN a2enmod ssl
RUN a2enmod rewrite

#disable default sites and use our app
RUN a2dissite 000-default
RUN a2dissite default-ssl
RUN a2ensite server
RUN a2ensite server-ssl

WORKDIR /srv/www/app

EXPOSE 80
EXPOSE 443

CMD ["/usr/sbin/apachectl","-DFOREGROUND"]
