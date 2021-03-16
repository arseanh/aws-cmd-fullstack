# Terminal Commands for fullstack course



## install commands

`sudo amazon-linux-extras install nginx1.12`: install nginx

`sudo amazon-linux-extras install postgresql9.6`: install psql

`sudo yum install git`: install git 

`sudo curl https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash`: install nvm

`npm install pm2 -g`: install pm2 globally 


## Basic commands
`sudo nano`: The text editor used in the Linux AMI 2.   

`sudo rm name-of-directory`: remove directory 

`cd name-of-directory`: change to directory

`ssh i- “keypair.pem” ec2-user@public-ip-address`: ssh into linux instance.  

`sudo chmod 777`: give all permissions to directory

`ls`: list files and folders in pwd

`pwd`: present working directory

`psql -d name-of-db -h host-name -p port -U username`: how to connect to the psql database


## npm/node commands
`node -v`: node version

`npm -v`: npm version 

`nvm -v`: node version manager version  

`nvm ls remote`: node versions available for download 

`nvm install version-of-node`: install node


## pm2 commands


`pm2 list`: list all running processes

`pm2 stop app 0`: stop app with id 0 

`pm2 delete app 0`: delete app with id 0 

`pm2 restart app 0`: restart app with id 0 

`pm2 start app.js -i max`: start app.js with max number of threads available


## nginx commands

`sudo systemctl restart nginx`: Restart nginx. After you modify the nginx.conf file you will need to restart nginx using this command otherwise your updates will not take place 

`sudo nano nginx.conf`: edit nginx file

`cd /etc/nginx`: cd into nginx directory

`sudo nano /var/log/nginx/error.log`: access error log

`sudo nginx -t`: test nginx

```
    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  localhost;


        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
                root /react-prod5/build;
                index index.html;                
                try_files $uri /index.html;

        }

        location /api/ {
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
                proxy_set_header X-NginX-Proxy true;
                proxy_pass http://10.0.1.187:3000;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;

        }
```

```
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/

worker_processes auto;
error_log /var/opt/rh/rh-nginx116/log/nginx/error.log;
pid /var/opt/rh/rh-nginx116/run/nginx/nginx.pid;

# Load dynamic modules. See /opt/rh/rh-nginx116/root/usr/share/doc/README.dynamic.
include /opt/rh/rh-nginx116/root/usr/share/nginx/modules/*.conf;

events {
    worker_connections  1024;
}

http {
    log_format  main  '"$http_x_forwarded_for" - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" ';

    access_log  /var/opt/rh/rh-nginx116/log/nginx/access.log  main;
    
    add_header Strict-Transport-Security "max-age=15768000; includeSubDomains" always;
    sendfile        on;
    tcp_nopush      on;
    tcp_nodelay     on;
    keepalive_timeout  65;
    types_hash_max_size 2048;
    

    include       /etc/opt/rh/rh-nginx116/nginx/mime.types;
    default_type  application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /opt/app-root/etc/nginx.d/*.conf;

    # Server block
    server {
        listen       8080 default_server;
        listen       [::]:8080 default_server;
        server_name  _;
        root         /opt/app-root/src;
        
        # Get the actual IP of the client through load balancer in the logs
        real_ip_header     X-Forwarded-For;
        set_real_ip_from   0.0.0.0/0;
        
        gzip on;
        gzip_vary on;
        gzip_comp_level    5;
        gzip_min_length 10240;
        gzip_proxied       any;
        gzip_types
            application/atom+xml
            application/javascript
            application/json
            application/rss+xml
            application/vnd.ms-fontobject
            application/x-font-ttf
            application/x-web-app-manifest+json
            application/xhtml+xml
            application/xml
            font/opentype
            image/svg+xml
            image/x-icon
            text/css
            text/plain
            text/x-component;

        # Load configuration files for the default server block.
        include      /opt/app-root/etc/nginx.default.d/*.conf;
        # Security headers
        add_header Strict-Transport-Security "max-age=15768000; includeSubDomains" always;
        add_header X-XSS-Protection "1; mode=block";
        add_header X-Frame-Options SAMEORIGIN;
        add_header X-Content-Type-Options nosniff;
        add_header X-Permitted-Cross-Domain-Policies "none";
        add_header Referrer-Policy "no-referrer-when-downgrade";
        
        location / {
            try_files $uri /index.html;
            add_header Cache-Control "no-store, no-cache, must-revalidate";
        }

        location ~* \.(?:manifest|appcache|html?|xml|json)$ {
            expires -1;
            add_header Cache-Control "no-store, no-cache";
        }
        # Media: images, icons, video, audio, HTC
        location ~* \.(?:jpg|jpeg|gif|png|ico|cur|gz|svg|svgz|mp4|ogg|ogv|webm|htc)$ {
          expires 1M;
          access_log off;
          add_header Cache-Control "public";
        }
        location ~* \.(?:css|js|woff2|ttf)$ {
            # try_files $uri =404;
            expires 1y;
            access_log off;
            add_header Cache-Control "public";
        }
        # Any route containing a file extension (e.g. /devicesfile.js)
        location ~ ^.+\..+$ {
            try_files $uri =404;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504  /50x.html;
            location = /50x.html {
        }

    }
}

```
