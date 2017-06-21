# learn
laern laravel


网页报错404配置在NGINX 上加配置
        location ~ \.php($|/){
                fastcgi_index   index.php;
                fastcgi_pass    127.0.0.1:9000;
                include         fastcgi_params;
                set $real_script_name $fastcgi_script_name;
                if ($real_script_name ~ "^(.+?\.php)(/.+)$") {
                    set $real_script_name $1;
                }
                fastcgi_split_path_info ^(.+?\.php)(/.*)$;
                fastcgi_param   PATH_INFO               $fastcgi_path_info;
                fastcgi_param   SCRIPT_NAME             $real_script_name;
                fastcgi_param   SCRIPT_FILENAME         $document_root$real_script_name;
                fastcgi_param   PHP_VALUE               open_basedir=$document_root:/tmp/:/proc/;
                access_log  /home/wwwlogs/access.log;
                error_log  /home/wwwlogs/nginx_error.log;
                }
        
        
        fastcgi_pass    127.0.0.1:9000; 加上这段配置后出现502错误,需要修该php-fpm
    [www]
      listen = 127.0.0.1:9000
      listen.backlog = -1
      listen.allowed_clients = 127.0.0.1
      listen.owner = www
      listen.group = www
      listen.mode = 0666
      user = www
      group = www
      pm = dynamic
      pm.max_children = 40
      pm.start_servers = 20
      pm.min_spare_servers = 20
      pm.max_spare_servers = 40
      request_terminate_timeout = 100
      request_slowlog_timeout = 0
      slowlog = var/log/slow.log
      在listen上要加上监听的9000端口号;
      
      
      
      
      
      
      
      
      
      
      
      
      Thinkphp 3.2 nginx路由去掉index.php ：
      location / {
            index  index.html index.htm index.php;
            #autoindex  on;
            #转发规则,支持url重写
            if (!-e $request_filename) {
                rewrite  ^(.*)$  /index.php?s=$1  last;
                break;
            }
        }
