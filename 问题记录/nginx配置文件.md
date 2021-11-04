```yml
server {
        listen       8081;
		# gzip config
		gzip on;
		gzip_min_length 1k;
		gzip_comp_level 9;
		gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
		gzip_vary on;
		gzip_disable "MSIE [1-6]\.";

		root /usr/share/nginx/html;
			
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;


		#后台服务配置，配置了这个location便可以通过http://域名/jeecg-boot/xxxx 访问		
		location ^~ /gsboot {
			proxy_pass              http://127.0.0.1:8080/gsboot/;
			proxy_set_header        Host 127.0.0.1;
			proxy_set_header        X-Real-IP $remote_addr;
			proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
		}
		#解决Router(mode: 'history')模式下，刷新路由地址不能找到页面的问题
		location / {
			# 用于配合 browserHistory使用
			try_files $uri $uri/ /index.html;
			
			root   html;
			index  index.html index.htm;
			if (!-e $request_filename) {
				rewrite ^(.*)$ /index.html?s=$1 last;
				break;
			}
		}

		#error_page  404              /404.html;
		# redirect server error pages to the static page /50x.html
		error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
		
		location /api {
			proxy_set_header   X-Forwarded-Proto $scheme;
			proxy_set_header   Host              $http_host;
			proxy_set_header   X-Real-IP         $remote_addr;
		}
        

        
        #
        
		
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }
```

