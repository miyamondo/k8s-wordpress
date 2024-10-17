FROM mysql:8.0.39-debian

ENV TZ Asia/Tokyo
ENV LC_ALL ja_JP.UTF-8
ENV MYSQL_ALLOW_EMPTY_PASSWORD no

RUN apt-get update && \
    apt-get install -y --no-install-recommends curl gnupg && \
    curl -k -fsSL https://repo.mysql.com/RPM-GPG-KEY-mysql-2023 | gpg --dearmor -o /usr/share/keyrings/example-archive-keyring.gpg

RUN apt-get install -y --no-install-recommends\
    locales \
    python3 \
    python-is-python3 \
    vim \
    && apt-get clean \
    && rm -rf /var/cache/apt/archives/* \
    /var/lib/apt/lists/*

RUN echo "ja_JP.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen ja_JP.UTF-8

# Setup MySQL
RUN touch /var/log/mysqld.log \
    && chown mysql:adm /var/log/mysqld.log

RUN mkdir /var/mysql \
    && chown mysql:adm /var/mysql

RUN mkdir -p /etc/mysql/conf.d/ && chown mysql:adm /etc/mysql/conf.d/

COPY ./my.cnf /etc/mysql/my.cnf