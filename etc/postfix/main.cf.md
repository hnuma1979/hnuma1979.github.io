# postfix/main.cf 参考設定

## base config
Postfixの基本設定です。

```conf
myhostname                        = mail2.example.jp
mydomain                          = mail2.example.jp
myorigin                          = $myhostname
mydestination                     = $myhostname, localhost, $mydomain
inet_interfaces                   = all
inet_protocols                    = ipv4
mynetworks                        = 127.0.0.0/32
home_mailbox                      = .mail/
smtpd_banner                      = $myhostname ESMTP
message_size_limit                = 10485760
mailbox_size_limit                = 1073741824
syslog_facility                   = mail
disable_vrfy_command              = yes
```

## smtpd_sasl
Postfixで SASL認証を有効化し、Dovecotを認証バックエンドとして利用するためのパラメータです。

```conf
smtpd_sasl_auth_enable            = yes
smtpd_sasl_type                   = dovecot
smtpd_sasl_path                   = private/auth
broken_sasl_auth_clients          = yes
```

## smtpd_recipient_restrictions
Postfix の SMTPセッションで RCPT TO（受信者）コマンドを処理する際に適用するアクセス制御リストです。

```conf
smtpd_recipient_restrictions      =
    permit_mynetworks
    permit_sasl_authenticated
    reject_unauth_destination
```

## smtpd_tls
Postfixで SMTPサーバにTLS（暗号化通信）を有効化するためのパラメータ です。Lets Encryptの証明書を使って安全なメール通信を実現しています。

```conf
smtpd_tls_cert_file               = /etc/letsencrypt/live/example.jp/fullchain.pem
smtpd_tls_key_file                = /etc/letsencrypt/live/example.jp/privkey.pem
smtpd_tls_session_cache_database  = btree:/var/lib/postfix/smtpd_scache
smtpd_tls_session_cache_timeout   = 3600s
smtpd_tls_received_header         = yes
smtpd_tls_loglevel                = 1
```

## smtpd_client_restrictions / smtpd_sender_restrictions
Postfix の SMTP接続時に適用するアクセス制御リストで、メール受信時のポリシーを定義する重要なパラメータです。

```conf
smtpd_client_restrictions         =
    reject_non_fqdn_sender
    reject_unknown_sender_domain
smtpd_sender_restrictions         =
    reject_non_fqdn_sender
    reject_unknown_sender_domain
```

## smtpd_milters
Postfix が SMTP 接続で受信したメールに対して適用する Milter の一覧を指定するパラメータです。

```conf
smtpd_milters =
        inet:localhost:8891
```

## non_smtpd_milters
Postfix が SMTP サーバプロセス以外でメールを処理する際に適用する Milter の一覧を指定するパラメータです。

```conf
non_smtpd_milters = $smtpd_milters
```


## milter_default_action
Postfix が Milter に接続できなかった場合や、Milter がエラーを返した場合にどのような動作をするかを指定するパラメータです。

```conf
milter_default_action = reject
# accept        配送優先
# tempfail      一時的なエラーとする
# reject        拒否
# quarantine    隔離
```

## header_chesks
メールヘッダーを正規表現でフィルタリングしたり、削除・追加するための仕組みです。スパム対策やポリシー適用に便利です。

```conf
header_checks = regexp:/etc/postfix/header_checks
```

### /etc/postfix/header_checks
```conf
/name=.*\.scr/ REJECT
/name=.*\.exe/ REJECT
：
```
