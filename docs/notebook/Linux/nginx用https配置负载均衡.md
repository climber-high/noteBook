# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;
    	

    upstream tomcats { 
    	server 39.98.151.5:8080;
	
    }

   # server {
   #     listen       80 default_server;
   #     listen       [::]:80 default_server;
   #     server_name  www.chilber-high.top;

          # Load configuration files for the default server block.
   #     include /etc/nginx/default.d/*.conf;

   #    location / {
   #   }

   #     error_page 404 /404.html;
   #         location = /40x.html {
   #     }

   #     error_page 500 502 503 504 /50x.html;
   #         	location = /50x.html {
   #     }
   # }

# Settings for a TLS enabled server.

    server {
        listen       443 ssl http2 default_server;
        listen       [::]:443 ssl http2 default_server;
        server_name  www.chilber-high.top;

        ssl_certificate "/etc/nginx/cert/3359362_chilber-high.top.pem";
        ssl_certificate_key "/etc/nginx/cert/3359362_chilber-high.top.key";
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers HIGH:!aNULL:!MD5;
	      ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
    		  proxy_pass http://tomcats;
        	proxy_redirect     off;
        	proxy_set_header   Host             $host;
        	proxy_set_header   X-Real-IP        $remote_addr;
        	proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        	proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        	proxy_max_temp_file_size 0;
        	proxy_connect_timeout      90;
        	proxy_send_timeout         90;
        	proxy_read_timeout         90;
        	proxy_buffer_size          4k;
        	proxy_buffers              4 32k;
        	proxy_busy_buffers_size    64k;
        	proxy_temp_file_write_size 64k;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

    server{
    	listen 80;
    	server_name www.chilber-high.top;
	    return 301 https://www.chilber-high.top;
	    # rewrite ^(.*)$ https://$host$1 permanent; 
    }
}

