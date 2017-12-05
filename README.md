# Multilingual Error Pages
Multilingual error pages for your HTTP server.

## Why
To replace generic server error messages with ones that have unique styling, branding and provide more useful information.

## NGINX
Inside `/etc/nginx/nginx.conf`:
```
http {
  ...

  # Language detection from header "Accept-Language"
  map $http_accept_language $lang {
    default 0;
    ~*^zh-hk     zh-hk;
    ~*^zh-mo     zh-hk;
    ~*^zh-tw     zh-hk;
    ~*^zh-hant   zh-hk;
    ~*^zh-yue    zh-hk;
    ~*^zh        zh-cn;

    ~*^ja        ja;
    ~*^cs        cs;
    ~*^sk        sk;
    ~*^nl        nl;
    ~*^ru        ru;
    ~*^it        it;
    ~*^pt        pt;
    ~*^es        es;
    ~*^fr        fr;
    ~*^de        de;

    ~*(^en|,en)  en;

    ~*,zh-hk     zh-hk;
    ~*,zh-mo     zh-hk;
    ~*,zh-tw     zh-hk;
    ~*,zh-hant   zh-hk;
    ~*,zh-yue    zh-hk;
    ~*,zh        zh-cn;

    ~*,ja        ja;
    ~*,cs        cs;
    ~*,sk        sk;
    ~*,nl        nl;
    ~*,ru        ru;
    ~*,it        it;
    ~*,pt        pt;
    ~*,es        es;
    ~*,fr        fr;
    ~*,de        de;
  }

  ...
}
```

Create new file `/etc/nginx/error-pages.conf`:
```
error_page  400 /error-400.html;
error_page  401 /error-401.html;
error_page  403 /error-403.html;
error_page  404 /error-404.html;
error_page  408 /error-408.html;
error_page  410 /error-410.html;
error_page  414 /error-414.html;
error_page  429 /error-429.html;
error_page  431 /error-431.html;
error_page  451 /error-451.html;
error_page  500 /error-500.html;
error_page  501 /error-501.html;
error_page  502 /error-502.html;
error_page  503 /error-503.html;
error_page  504 /error-504.html;


location ^~ /error- {
  internal;
  root /srv/www/ERROR-PAGES/$lang;
}

location ^~ /maintenance- {
  internal;
  root /srv/www/ERROR-PAGES/$lang;
  add_header Retry-After "604800";
}
```

Within individual site (server) configuration files:
```
server {
  ...

  include /etc/nginx/error-pages.conf;

  ...
}
```
