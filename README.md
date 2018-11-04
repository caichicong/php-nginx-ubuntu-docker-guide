
## nginx配置

```
    server {
      listen  80 default_server;
      server_name _;
      root   /usr/share/nginx/html;
    
      location / {
       index index.html index.htm index.php;
       autoindex off;
      }
    
      location ~ \.php(.*)$ {
       root   /var/www/html/;
       fastcgi_pass 172.17.0.2:9000;
       fastcgi_index index.php;
       fastcgi_split_path_info ^((?U).+\.php)(/?.+)$;
       fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
       fastcgi_param PATH_INFO $fastcgi_path_info;
       fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;
       include  fastcgi_params;
      }
    }
```

注意IP 172.17.0.2 与下面的数据库建立命令里的IP对应

## mysql建立新的用户

CREATE USER 'zhuangxiu'@'172.17.0.2' IDENTIFIED BY 'password';

GRANT ALL PRIVILEGES ON `zhuangxiu`.* TO 'zhuangxiu'@'172.17.0.2' ;

## mysql my.cnf配置修改

    bind-address		= 0.0.0.0


## php代码

/docker/www/db.php

```$php
$dbh = new PDO('mysql:host=172.17.0.1;dbname=zhuangxiu', 'zhuangxiu', 'password');
$dbh->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
$dbh->exec('set names utf8');

$rs = $dbh->query("select * from agent_config");
```

/docker/www/info.php

```$xslt
<?php phpinfo();?>
```


