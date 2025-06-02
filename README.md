# SSL Certification Expiration Checker:

ssl-cert-check.sh is a Bourne shell script that can be used to report on expiring SSL certificates. The script was designed to be run from cron and can e-mail warnings, send Telegram notifications, or log alerts through nagios.  

# Usage:
<pre>
$ ./ssl-cert-check.sh
Usage: ./ssl-cert-check.sh [ -e email address ] [ -E sender email address ] [ -x days ] [-q] [-a] [-b] [-h] [-i] [-n] [-N] [-v]
       { [ -s common_name ] && [ -p port] } || { [ -f cert_file ] } || { [ -c cert file ] } || { [ -d cert dir ] }"

  -a                : Send a warning message through E-mail
  -b                : Will not print header
  -c cert file      : Print the expiration date for the PEM or PKCS12 formatted certificate in cert file
  -d cert directory : Print the expiration date for the PEM or PKCS12 formatted certificates in cert directory
  -e E-mail address : E-mail address to send expiration notices
  -E E-mail address : Sender E-mail address
  -f cert file      : File with a list of FQDNs and ports
  -h                : Print this screen
  -i                : Print the issuer of the certificate
  -k password       : PKCS12 file password
  -n                : Run as a Nagios plugin
  -N                : Run as a Nagios plugin and output one line summary (implies -n, requires -f or -d)
  -p port           : Port to connect to (interactive mode)
  -s commmon name   : Server to connect to (interactive mode)
  -t type           : Specify the certificate type
  -q                : Don't print anything on the console
  -v                : Specify a specific protocol version to use (tls, ssl2, ssl3)
  -V                : Only print validation data
  -x days           : Certificate expiration interval (eg. if cert_date < days)
  -T                : Enable Telegram notifications
  -B token          : Telegram Bot token
  -C chat_id        : Telegram Chat ID
</pre>

# Examples:

Print the expiration times for one or more certificates listed in ssldomains:

<pre>
$ ssl-cert-check.sh -f ssldomains
Host                                            Status       Expires      Days Left
----------------------------------------------- ------------ ------------ ----------
www.prefetch.com:443                            Valid        May 23 2006  218
mail.prefetch.net:993                           Valid        Jun 20 2006  246
gmail.google.com:443                            Valid        Jun 7 2006   233
www.sun.com:443                                 Valid        May 11 2009  1302
www.spotch.com:443                              Connection refused Unknown Unknown
</pre>

Check all certificates with file pattern "/etc/haproxy/ssl/\*.pem"

<pre>
$ ssl-cert-check.sh -d "/etc/haproxy/ssl/*.pem"
Host                                            Status       Expires      Days
----------------------------------------------- ------------ ------------ ----
FILE:/etc/haproxy/ssl/example1.org.pem      Valid        Jan 6 2017   78                                 
FILE:/etc/haproxy/ssl/example2.org.pem      Valid        Jan 1 2017   73                                 
FILE:/etc/haproxy/ssl/example3.org.pem      Valid        Jan 6 2017   78                                 
</pre>

Send Telegram notifications for expiring certificates:

<pre>
$ ssl-cert-check.sh -aTB "your_bot_token" -C "your_chat_id" -f ssldomains -x 30
</pre>

Send an e-mail to admin@prefetch.net if a domain listed in ssldomains will expire in the next 60-days:

<pre>
$ ssl-cert-check.sh -a -f ssldomains -q -x 60 -e admin@prefetch.net
</pre>

# Features

* Certificate expiration monitoring
* Email notifications
* Telegram notifications
* Hostname verification against certificate CN and SANs
* Support for various STARTTLS services:
  - SMTP (25, 587)
  - IMAP (143)
  - POP3 (110)
  - FTP (21)
  - XMPP (5222)
  - IRC (194)
  - Postgres (5432)
  - MySQL (3306)
  - LMTP (24)
  - NNTP (119)
  - SIEVE (4190)
  - LDAP (389)
* Support for PEM and PKCS12 certificates
* Nagios plugin support
* Batch processing of multiple certificates

# Additional Documentation

Documentation And Examples: http://prefetch.net/articles/checkcertificate.html
