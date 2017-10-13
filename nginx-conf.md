server {
    listen      *:80;
    server_name ewiki.localhost;
    root        /home/users/Wikitten;
    index       index.php;
    resolver    8.8.8.8;

    error_log   /home/users/nginx_log/nginx_ewiki_error_80.log;
    access_log  /home/users/nginx_log/nginx_ewiki_access_80.log;

    location ~* ^/static/(css|js|img|fonts)/.+.(jpg|jpeg|gif|css|png|js|ico|html|xml|txt|swf|pdf|txt|bmp|eot|svg|ttf|woff|woff2)$ {
            access_log        off;
            expires           max;

    }

    location / {
            rewrite ^(.*)$ /index.php last;
#index index.php;
#autoindex on;
    }

    location ~ ^/assets/.*\.php$ {
        deny all;
    }

    location ~ \.php$ {
        include             fastcgi_params;
        fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
        #fastcgi_pass   127.0.0.1:9000;
        fastcgi_pass    unix:/usr/local/php7/var/run/users.sock;
        try_files           $uri =404;
    }

    location ~* /\. {
        deny all;
    }
}
