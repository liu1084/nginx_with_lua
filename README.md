# nginx_with_lua

## 1. nginx docker with lua and openresty.

## 2. module include:
load_module /usr/lib/nginx/modules/ndk_http_module.so;
load_module /usr/lib/nginx/modules/ngx_http_auth_spnego_module.so;
load_module /usr/lib/nginx/modules/ngx_http_brotli_filter_module.so;
load_module /usr/lib/nginx/modules/ngx_http_brotli_static_module.so;
load_module /usr/lib/nginx/modules/ngx_http_encrypted_session_module.so;
load_module /usr/lib/nginx/modules/ngx_http_geoip2_module.so;
load_module /usr/lib/nginx/modules/ngx_http_geoip_module.so;
load_module /usr/lib/nginx/modules/ngx_http_headers_more_filter_module.so;
load_module /usr/lib/nginx/modules/ngx_http_image_filter_module.so;
load_module /usr/lib/nginx/modules/ngx_http_js_module.so;
load_module /usr/lib/nginx/modules/ngx_http_lua_module.so;
load_module /usr/lib/nginx/modules/ngx_http_modsecurity_module.so;
load_module /usr/lib/nginx/modules/ngx_http_passenger_module.so;
load_module /usr/lib/nginx/modules/ngx_http_set_misc_module.so;
load_module /usr/lib/nginx/modules/ngx_http_subs_filter_module.so;
load_module /usr/lib/nginx/modules/ngx_http_xslt_filter_module.so;
load_module /usr/lib/nginx/modules/ngx_stream_geoip2_module.so;
load_module /usr/lib/nginx/modules/ngx_stream_geoip_module.so;
load_module /usr/lib/nginx/modules/ngx_stream_js_module.so;
load_module /usr/lib/nginx/modules/ngx_stream_lua_module.so;

nginx docker as :

[docker file]: https://raw.githubusercontent.com/nginxinc/docker-nginx/master/modules/Dockerfile



## 3. lua use example

```lua
rewrite_by_lua_block {
    local red = redis:new()
    red:set_timeouts(1000, 1000, 1000)
    local host = '172.16.0.87'
    local port = 6380
    local password = '****';
    local ok, err = red:connect(host, port)
    red:auth(password)

    if ok then
        ok, err = red:hset('REQUEST_ID', ngx.var.request_id, 1)
        if err then
            ngx.say(err)
            return
        end
        if ok then
            red:expire('REQUEST_ID', 60)
        end
    end
}
```

