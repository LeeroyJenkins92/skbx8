FROM nginx
COPY nginx.conf /etc/nginx/
ARG CHANGE_SOURCE=false
RUN if [ ${CHANGE_SOURCE} = true ]; then \
    # Change application source from dl-cdn.alpinelinux.org to aliyun source
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories \
;fi
#Attach www-data user to an already existing group with -Gy
RUN apt update \
    && apt upgrade -y \
    && apt install libssl-dev \
    && apt install bash-completion

RUN if [ `grep -c www-data /etc/group` = 0 ]; then \
         adduser -D -H -u 1000 -s /bin/bash www-data; \
    else \
         adduser -D -H -u 1000 -s /bin/bash www-data -G www-data; \
    fi

ADD ./startup.sh /opt/startup.sh
RUN sed -i 's/\r//g' /opt/startup.sh
CMD ["/bin/bash", "/opt/startup.sh"]
EXPOSE 80 443
