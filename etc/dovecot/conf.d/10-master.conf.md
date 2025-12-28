# dovecot/conf.d/10-master.conf

## imap-login
port 993 の有効化

```conf
service imap-login {
  inet_listener imap {
    port = 0
  }
  inet_listener imaps {
    port = 993
    ssl = yes
  }
}
```

## pop3-login
port 995 の有効化

```conf
service pop3-login {
  inet_listener pop3 {
    port = 0
  }
  inet_listener pop3s {
    port = 995
    ssl = yes
  }
}
```

## submission-login
port 587 の有効化

```conf
service submission-login {
  inet_listener submission {
    port = 587
  }
}
```

## auth
認証設定

```conf
service auth {
  unix_listener auth-userdb {
    mode = 0666
    user = postfix
    group = postfix
  }

  unix_listener /var/spool/postfix/private/auth {
    mode = 0666
  }
}
```
