# dovecot/conf.d/10-auth.conf

## auth_username_format
認証時にクライアントから送信されたユーザー名をどのように変換するかを指定します。

```conf
auth_username_format = %n
# %n → ユーザー名部分   （@より前）
# %d → ドメイン部分     （@より後）
# %u → 完全なユーザー名
```

## auth_mechanisms
認証メカニズムを指定します。
```conf
auth_mechanisms = plain
```

## disable_plaintext_auth
平文認証を禁止する設定
```conf
disable_plaintext_auth = yes
```

## auth_mechanisms
Dovecot の認証メカニズムを指定する設定項目です。クライアントがログインする際に、どの認証方式をサポートするかを定義します。

```conf
auth_mechanisms = plain login
```
