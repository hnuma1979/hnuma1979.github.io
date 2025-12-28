# postfix/master.cf の設定

## smtp
外部からのメール受信に使用される。
```conf
smtp      inet  n       -       n       -       -       smtpd
```

## submission
OBP25対応。外部からのメール受信に使用される。

```conf
submission inet n       -       n       -       -       smtpd
   -o syslog_name=postfix/submission
   -o smtpd_sasl_auth_enable=yes
   -o smtpd_tls_auth_only=yes
   -o smtpd_relay_restrictions=permit_sasl_authenticated,reject
```

## smtps
SSL対応。外部からのメール受信に使用される。

```conf
smtps     inet  n       -       n       -       -       smtpd
   -o syslog_name=postfix/smtps
   -o smtpd_tls_wrappermode=yes
   -o smtpd_sasl_auth_enable=yes
   -o smtpd_relay_restrictions=permit_sasl_authenticated,reject
```
