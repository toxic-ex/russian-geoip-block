Install geoip nginx module + geoip database
`sudo apt-get install libnginx-mod-http-geoip geoip-database`

Add this to `/etc/nginx/nginx.conf`, inside the `http {` section:
```
        geoip_country /usr/share/GeoIP/GeoIP.dat;
        map $geoip_country_code $allowed_country {
                default yes;
                RU no;
        }
```

Add this to `/etc/nginx/sites-enabled/*`:
```
  location /test-ru {
    add_header "Content-Type" "text/plain; charset=utf-8" always;
    return 403 "Русский военный корабль, иди на хуй";
  }

  location / {
    if ($allowed_country = no) {
        add_header "Content-Type" "text/plain; charset=utf-8" always;
        return 403 "Русский военный корабль, иди на хуй";
    }
```

Reload nginx:
`nginx -t && systemctl reload nginx`

Now point your browser to https://your-site/test-ru

You should see a message in Russian.
