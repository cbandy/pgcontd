# Authentication

https://www.postgresql.org/docs/current/client-authentication.html <br/>
https://www.postgresql.org/docs/current/runtime-config-file-locations.html

| Parameter | | |
|-----------|-|-|
| `hba_file` | S | `pg_hba.conf`
| `ident_file` | S | `pg_ident.conf`

```
${PGDATA}/pg_hba.conf
${PGDATA}/pg_ident.conf
```

| Method | |
|--------|-|
| `peer` | OS user, Unix domain socket only
| `cert` | [TLS](TLS.md) certificates, TCP only
| `gss` | Kerberos, Active Directory, TCP only
| `scram-sha-256` | Password, according to [RFC 7677][]
| `md5` | Password, long-supported but weaker hash

[RFC 7677]: https://tools.ietf.org/html/rfc7677

- No `trust`; PostgreSQL must perform some authentication.
