# N.P.M. (Nginx PHP MariaDB)
This repository contains a lightweight docker container setup for PHP applications development.

After executing `docker compose up -d`, 2 containers will be created:
* `npm-webserver-1` (size: approx. 132 MB)
* `npm-maria-1` (size: approx: 407 MB)

If you have changed the service names in the `docker-compose.yml` file, the container names may be according your naming.

The container `npm-webserver` comes with pre-installed / configured Nginx web server and PHP 8.3 running on Alpine Linux, while the container `npm-maria-1` is the standard MariaDB v11.5 docker image.

## SSL
To use HTTPS you need to generate an SSL cert + key, plus the Diffie-Hellman params (one-time only).
**Optional:** To avoid the invalid cert warning in Chrome/Chromium, you can import the generated cert in your web browser.

### 1. Generate SSL cert + key and Diffie-Hellman params
Generating SSL cert
```shell
user@machine:~/npm$ openssl req -x509 -nodes -days 365 -subj "/C=CA/ST=QC/O=MyLocalAuthority/CN=npm.local" -addext "subjectAltName=DNS:npm.local" -newkey rsa:2048 -keyout ./docker/config/ssl/npm.local.key -out ./docker/config/ssl/npm.local.cert;
```

You only need to (re-)create the SSL certs, if the shipped one are expired or not working.

Generating Diffie-Hellman params
```shell
user@machine:~/npm$ openssl dhparam -out ./docker/config/ssl/dhparam.pem 4096
```

To avoid the invalid cert warning in Chrome/Chromium, you can import the generated cert (`npm-test.cert`) into the web browser.
1. Open the browser and navigate to `chrome://settings/certificates`
2. Here you go on the tab "Authorities" and then click on the button "Import"
3. Navigate to the project directory `docker/config/ssl` and open the previously generated certificate `npm-test.cert`

DONE!
