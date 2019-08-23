### 静态资源web访问
> 静态资源应用场景    CDN

>文件读取
    Syntax: sendfile on | off;
    Default: sendfile off;
    Context: http, server, location, if in location

    tcp_nopush:
        Syntax: tcp_nopush on | off;
        Default:tcp_nopush off;
        Context:http, server, location

    tcp_nodelay
        Syntax: tcp_nodelay on | off;
        Default:tcp_nodelay on;
        Context:http, server, location;
    作用： keepalive连接下，提高网络包的传输实时性

>压缩

    Syntax:gzip on | off;
    Default:gzip off;
    Context:http, server, location, if in location
    压缩传输

    Syntax: gzip_comp_level level;
    Default:gzip_comp_level 1;
    Context:http, server, location;

    Syntax:  gzip_http_version 1.0 | 1.1
    Default: gzip_http_version 1.1;
    Context: http, server, location
>拓展nginx压缩模块

    http_gzip_static_module -预读gzip功能
    http_gunzip_module -应用支持gunzip的压缩方式

    location ~ ^/download{
        gzip_static on;
        tcp_nopush on;
        root /root;
    }

>
>

### 代理服务

### 负载君合调度器SLB

### 动态缓存