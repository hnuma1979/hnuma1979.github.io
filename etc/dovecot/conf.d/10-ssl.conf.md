# dovecot/conf.d/10-ssl.conf

Lets Encript の証明書を設定

```conf
ssl_cert = </etc/letsencrypt/live/example.jp/fullchain.pem
ssl_key = </etc/letsencrypt/live/example.jp/privkey.pem
```