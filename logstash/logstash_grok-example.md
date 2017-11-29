
```
  grok {
    match => [ "message" , '\A%{IP:remote_addr}%{SPACE}\-%{SPACE}%{IP:forwarded_for}(?:\,%{SPACE}%{IP:IP:forwarded_for_2})?%{SPACE}\-%{SPACE}%{GREEDYDATA:remote_user}%{SPACE}\[%{HTTPDATE:sourcetimestamp}]%{SPACE}\"%{WORD:http-method}%{SPACE}%{GREEDYDATA:http-request}%{SPACE}HTTP/%{NUMBER:http-version}\"%{SPACE}%{NUMBER:http-status}%{SPACE}%{NUMBER:http-bytesSent}%{SPACE}upstream%{SPACE}time\/addr%{SPACE}%{GREEDYDATA:upstreamfoo}%{SPACE}\"%{GREEDYDATA:http-referer}\"%{SPACE}\"%{GREEDYDATA:http-userAgent}\"%{SPACE}\"%{GREEDYDATA:gzip_ratio}\"%{SPACE}\"%{DATA:cookie-jsessionid}%{SPACE}\-(?:\-|%{SPACE}%{GREEDYDATA:cookie-route})?\"']
    overwrite => [ "message" ]
  }
```

# extract from nginx config:
#
#   log_format gzip '$remote_addr - $http_x_forwarded_for - $remote_user [$time_local]  '
#                    '"$request" $status $bytes_sent upstream time/addr $upstream_response_time/$upstream_addr '
#                    '$upstream_cache_status '
#                    '"$http_referer" "$http_user_agent" "$gzip_ratio" "$cookie_JSESSIONID - $cookie_route"';
#
#   04/Apr/2017:16:39:34·+0200
#   dd/MMM/yyyy:HH:mm:ss +Z



```
90.90.230.80 - 90.90.230.80 - - [04/Apr/2017:16:39:34 +0200]  "POST /url-1 HTTP/1.1" 200 651 upstream time/addr 1.008/127.0.0.1:8080 - "http://long-url" "Mozilla/5.0 (Windows NT 5.1) AppleWebKit/534.24 (KHTML, like Gecko) Chrome/0.0.0.0 Safari/534.24" "-" "39085D6FB967681942373BFEA5FBC99D.i01 - -"
```

