# magento-php5.3
Instructions and patches for running a legacy version of Magento that requires PHP 5.3

## WARNING

THIS IS FOR USING A VERSION OF PHP WITH KNOWN SECURITY ISSUES AND OUTSIDE OF MAINTAINENCE. THE VERSIONS OF MAGENTO THIS WILL RUN ARE LARGELY ALSO OUT OF THIER SUPPORT WINDOW AND HORRIBILY FLAWED AND SHOULD NOT BE USED WITHOUT PROPER PATCHING THAT IS WELL OUTSIDE OF THE OBJECTIVE OF THESE INSTRUCTIONS.

## Objective

Install a legacy version of PHP to run an equally legacy version of Magento. You will end up with a FPM and CLI version of PHP.

## Prerequisites

Following instructions relevant to your legacy Magento install will get you the supporting libraries you need. As an alternative, you can run the _./configure_ command and it will tell you what you are missing. You can resolve it with your system package manager or other means.

## Sources

Start with PHP 5.3.29 sources from php.net

## Patch

Patch with the included patch file (_php-5.3.29.patch_). The fixes are as follows:

* Correct compile error seen in mbstring when compiling with Oniguruma 6.9.2 and GCC 8.3.0. Patch taken nearly verbatim from a commit fixing a bug in PHP7

## Configure, make

```
./configure '--prefix=/usr' '--build=x86_64-pc-linux-gnu' '--host=x86_64-pc-linux-gnu' '--mandir=/usr/share/man' '--infodir=/usr/share/info' '--datadir=/usr/share' '--sysconfdir=/etc' '--localstatedir=/var/lib' '--prefix=/usr/lib64/php5.3' '--mandir=/usr/lib64/php5.3/man' '--infodir=/usr/lib64/php5.3/info' '--libdir=/usr/lib64/php5.3/lib' '--with-libdir=lib64' '--without-pear' '--disable-maintainer-zts' '--enable-bcmath' '--with-bz2' '--enable-calendar' '--enable-ctype' '--with-curl' '--without-curlwrappers' '--enable-dom' '--without-enchant' '--disable-exif' '--enable-fileinfo' '--enable-filter' '--enable-ftp' '--with-gettext' '--without-gmp' '--enable-hash' '--with-mhash' '--with-iconv' '--disable-intl' '--enable-ipv6' '--enable-json' '--without-kerberos' '--enable-libxml' '--enable-mbstring' '--with-mcrypt' '--without-mssql' '--with-onig=/usr' '--with-openssl' '--with-openssl-dir=/usr' '--disable-pcntl' '--enable-phar' '--enable-pdo' '--without-pgsql' '--enable-posix' '--without-pspell' '--without-recode' '--enable-simplexml' '--disable-shmop' '--without-snmp' '--enable-soap' '--disable-sockets' '--without-sqlite' '--without-sqlite3' '--without-sybase-ct' '--disable-sysvmsg' '--disable-sysvsem' '--disable-sysvshm' '--without-tidy' '--enable-tokenizer' '--disable-wddx' '--enable-xml' '--enable-xmlreader' '--enable-xmlwriter' '--without-xmlrpc' '--without-xsl' '--enable-zip' '--with-zlib' '--disable-debug' '--enable-dba' '--without-cdb' '--with-db4' '--disable-flatfile' '--with-gdbm' '--disable-inifile' '--without-qdbm' '--without-freetype-dir' '--without-t1lib' '--disable-gd-jis-conv' '--with-jpeg-dir=/usr' '--with-png-dir=/usr' '--without-xpm-dir' '--with-gd' '--with-mysql=/usr' '--with-mysql-sock=/var/run/mysqld/mysqld.sock' '--with-mysqli=/usr/bin/mysql_config' '--without-pdo-dblib' '--with-pdo-mysql=/usr' '--without-pdo-pgsql' '--without-pdo-sqlite' '--without-pdo-odbc' '--with-readline' '--without-libedit' '--without-mm' '--with-pic' '--with-pcre-regex=/usr' '--with-pcre-dir=/usr' '--with-config-file-path=/etc/php/fpm-php5.3' '--with-config-file-scan-dir=/etc/php/fpm-php5.3/ext-active' '--disable-embed' '--enable-cli' '--disable-cgi' '--enable-fpm' '--without-apxs2'
```

```
make
make install
```

## Post-install

Make the versioned symlinks.

```
cd /usr/bin
ln -s /usr/lib64/php5.3/bin/php
ln -s /usr/lib64/php5.3/bin/php-config
ln -s /usr/lib64/php5.3/sbin/php-fpm
ln -s /usr/lib64/php5.3/bin/phpize
```

memcache (optional)

```
wget https://pecl.php.net/get/memcache-2.2.7.tgz
tar xzf memcache-2.2.7.tgz
cd memcache-2.2.7
phpize
./configure
make
make install
```

APC (technically optional, but do install it)

```
wget https://pecl.php.net/get/APC-3.1.13.tgz
tar xzf APC-3.1.13.tgz
cd APC-3.1.13
phpize
./configure
make
make install
```