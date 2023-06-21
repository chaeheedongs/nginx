# Nginx







## Index

* [Intro](#intro)
* [Configuration Context](#configuration-context)
* [Director](#director)
* [Basic Commands](#basic-commands)
* [Configuration File Path](#configuration-file-path)

---







## intro

* [Nginx 홈페이지 바로가기](https://nginxstore.com/training/nginx-conf-%EC%84%A4%EC%A0%95-context-logic-%EB%B0%B0%EC%9A%B0%EA%B8%B0/)







## configuration context

* 컨텍스트와 지시어의 조합으로 구성된다.
* 컨텍스트에는 Main, Events, HTTP, Stream등이 포함된다.
* Main
  * 구성 Worker 프로세스 수, Linux 사용자명, 구성 파일, 로그 파일 위치등 모든 항목의 액세스 권한 등을 선언
* Events
  * 각 Worker 프로세스에 할당된 작업 프로세스당 연결 수를 관리
* HTTP
  * NGINX가 HTTP 및 HTTPS 연결을 처리하는 방법을 정의
  * Ex) 백단 또는 어플리케이션 풀을 설정
* HTTP -> Server
  * Server 는 가성 서버다.
  * 프로세스가 HTTP 요청을 받는 가상 호스트라고도 하며, 도메인 이름이거나 IP, Unit Socket일 수 있다.
* HTTP -> Server -> Location
  * 특정 URI를 기반으로 가상 서버가 HTTP 요청을 처리하는 방법을 정의한다.
* HTTP -> Upstream
  * 어플리케이션 서비스 그룹을 정의한다.
  * 기본적으로 로드 밸런싱으로 사용한다.
* Stream
  * NGINX가 Layer3과 Layer4 TCP / UDP와 같은 트래픽을 처리하는 방법을 정의한다.

```

Main
l  l- Events
l  l- HTTP
l     l- Server
l     l  l- Location
l     l- Upstream
l  l- Stream
l     l- Server
l     l- Upstream
```







## director

* 지시어는 NGINX동작을 제어하는 단일 명령문이다.

```
Server {
	# Listen Directive : NGINX가 80번 포트로 수신하도록 지시
	listen 80;
	
	# Root Directive : 파일 시스템의 경로를 지정
	root /usr/share/nginx/html;
}
```







## basic commands

```
# 현재 NGINX 버전을 확인할 수 있다.
/ # nginx -v
nginx version: nginx/1.25.1

# --------------------------------------------


# NGINX 설정 구성에 문제가 없는지 구문 유효성을 체크한다.
/ # nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful



# 현재 NGINX 인스턴스에서 구현된 구성을 확인할 수 있다.
/ # nginx -T
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
# configuration file /etc/nginx/nginx.conf:

user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}

# configuration file /etc/nginx/mime.types:

types {
    text/html                                        html htm shtml;
    text/css                                         css;
    text/xml                                         xml;
    image/gif                                        gif;
    image/jpeg                                       jpeg jpg;
    application/javascript                           js;
    application/atom+xml                             atom;
    application/rss+xml                              rss;

    text/mathml                                      mml;
    text/plain                                       txt;
    text/vnd.sun.j2me.app-descriptor                 jad;
    text/vnd.wap.wml                                 wml;
    text/x-component                                 htc;

    image/avif                                       avif;
    image/png                                        png;
    image/svg+xml                                    svg svgz;
    image/tiff                                       tif tiff;
    image/vnd.wap.wbmp                               wbmp;
    image/webp                                       webp;
    image/x-icon                                     ico;
    image/x-jng                                      jng;
    image/x-ms-bmp                                   bmp;

    font/woff                                        woff;
    font/woff2                                       woff2;

    application/java-archive                         jar war ear;
    application/json                                 json;
    application/mac-binhex40                         hqx;
    application/msword                               doc;
    application/pdf                                  pdf;
    application/postscript                           ps eps ai;
    application/rtf                                  rtf;
    application/vnd.apple.mpegurl                    m3u8;
    application/vnd.google-earth.kml+xml             kml;
    application/vnd.google-earth.kmz                 kmz;
    application/vnd.ms-excel                         xls;
    application/vnd.ms-fontobject                    eot;
    application/vnd.ms-powerpoint                    ppt;
    application/vnd.oasis.opendocument.graphics      odg;
    application/vnd.oasis.opendocument.presentation  odp;
    application/vnd.oasis.opendocument.spreadsheet   ods;
    application/vnd.oasis.opendocument.text          odt;
    application/vnd.openxmlformats-officedocument.presentationml.presentation
                                                     pptx;
    application/vnd.openxmlformats-officedocument.spreadsheetml.sheet
                                                     xlsx;
    application/vnd.openxmlformats-officedocument.wordprocessingml.document
                                                     docx;
    application/vnd.wap.wmlc                         wmlc;
    application/wasm                                 wasm;
    application/x-7z-compressed                      7z;
    application/x-cocoa                              cco;
    application/x-java-archive-diff                  jardiff;
    application/x-java-jnlp-file                     jnlp;
    application/x-makeself                           run;
    application/x-perl                               pl pm;
    application/x-pilot                              prc pdb;
    application/x-rar-compressed                     rar;
    application/x-redhat-package-manager             rpm;
    application/x-sea                                sea;
    application/x-shockwave-flash                    swf;
    application/x-stuffit                            sit;
    application/x-tcl                                tcl tk;
    application/x-x509-ca-cert                       der pem crt;
    application/x-xpinstall                          xpi;
    application/xhtml+xml                            xhtml;
    application/xspf+xml                             xspf;
    application/zip                                  zip;

    application/octet-stream                         bin exe dll;
    application/octet-stream                         deb;
    application/octet-stream                         dmg;
    application/octet-stream                         iso img;
    application/octet-stream                         msi msp msm;

    audio/midi                                       mid midi kar;
    audio/mpeg                                       mp3;
    audio/ogg                                        ogg;
    audio/x-m4a                                      m4a;
    audio/x-realaudio                                ra;

    video/3gpp                                       3gpp 3gp;
    video/mp2t                                       ts;
    video/mp4                                        mp4;
    video/mpeg                                       mpeg mpg;
    video/quicktime                                  mov;
    video/webm                                       webm;
    video/x-flv                                      flv;
    video/x-m4v                                      m4v;
    video/x-mng                                      mng;
    video/x-ms-asf                                   asx asf;
    video/x-ms-wmv                                   wmv;
    video/x-msvideo                                  avi;
}

# configuration file /etc/nginx/conf.d/default.conf:
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

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



# NGINX를 재 시작 한다.
/ # nginx -s reload
2023/06/21 15:22:45 [notice] 63#63: signal process started



```







## configuration file path

```
/etc/nginx # pwd
/etc/nginx


/etc/nginx # ls -al
total 52
drwxr-xr-x    1 root     root          4096 Jun 15 02:58 .
drwxr-xr-x    1 root     root          4096 Jun 21 15:18 ..
drwxr-xr-x    1 root     root          4096 Jun 21 15:18 conf.d
-rw-r--r--    1 root     root          1077 Jun 13 17:34 fastcgi.conf
-rw-r--r--    1 root     root          1007 Jun 13 17:34 fastcgi_params
-rw-r--r--    1 root     root          5349 Jun 13 17:34 mime.types
lrwxrwxrwx    1 root     root            22 Jun 15 02:58 modules -> /usr/lib/nginx/modules
-rw-r--r--    1 root     root           648 Jun 13 17:34 nginx.conf
-rw-r--r--    1 root     root           636 Jun 13 17:34 scgi_params
-rw-r--r--    1 root     root           664 Jun 13 17:34 uwsgi_params


/etc/nginx # ls -al conf.d
total 20
drwxr-xr-x    1 root     root          4096 Jun 21 15:18 .
drwxr-xr-x    1 root     root          4096 Jun 15 02:58 ..
-rw-r--r--    1 root     root          1093 Jun 21 15:18 default.conf
```

* Main File
  * /etc/nginx/nginx.conf
  * (주의) NGINX는 알파벳 순으로 지시문을 리딩한다.
* Includes
  * /etc/nginx/conf.d/*.conf
  * NGINX 인스턴스를 구성하는 것은 모든 Includes 설정 파일을 conf.d 디렉토리 하위에 넣는 것 이다.
  * /etc/nginx/nginx.conf 파일의 include 지시문을 사용해 구성을 조립 한다.

