FROM httpd:2.4
RUN apt-get update
COPY ./index.html /usr/local/apache2/htdocs/
EXPOSE 80
ENTRYPOINT ["/usr/local/apache2/bin/httpd", "-D", "FOREGROUND"]
