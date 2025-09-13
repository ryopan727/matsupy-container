FROM nextcloud:stable-apache
# SMB接続有効
RUN apt-get update && apt-get install -y libsmbclient-dev smbclient && pecl install smbclient && docker-php-ext-enable smbclient
