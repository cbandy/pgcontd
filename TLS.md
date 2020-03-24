# TLS certificates

https://www.postgresql.org/docs/current/ssl-tcp.html

| Parameter | | |
|-----------|-|-|
| `ssl` | CF | `off`, must be `on`
| `ssl_cert_file` | CF | `server.crt`, server certificate bundle
| `ssl_key_file` | CF | `server.key`, server private key
| `ssl_ca_file` | CF | trusted authorities; verify clients
| `ssl_crl_file` | CF | revocation list; reject clients

- Watch certificate files for changes.
- Validate certificate files before reload.

```
/etc/pgcontd/tls/server/tls.crt
/etc/pgcontd/tls/server/tls.key
/etc/pgcontd/tls/server.crt.d/*.{crt,pem}
/etc/pgcontd/tls/ca/tls.crt
/etc/pgcontd/tls/ca/tls.crl
/etc/pgcontd/tls/ca.crt.d/*.{crt,pem}
/etc/pgcontd/tls/ca.crl.d/*.{crl,pem}

(data_directory)/server.crt
(data_directory)/server.key
(data_directory)/ca.crt
(data_directory)/ca.crl
```

## Scope creep

- Disconnect clients authenticated with revoked certificates?
- Use different CAs to authenticate different clients?
