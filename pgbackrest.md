# pgBackRest

https://pgbackrest.org

| Command | PostgreSQL | Repository |
|---------|------------|------------|
| `archive-get`    |yes| -
| `archive-push`   |yes| -
| `backup`         | - |yes
| `check`          |yes|yes
| `expire`         | - |yes
| `info`           |yes|yes
| `restore`        |yes| -
| `stanza-create`  | - |yes
| `stanza-upgrade` | - |yes

```
/etc/pgbackrest/pgbackrest.conf
/etc/pgbackrest/conf.d/*.conf
```

- Ensure archiving is enabled.
- Ensure stanza and paths agree.
- Ensure connectivity to/from remote repository.

```ini
# postgresql.conf (primary, standby)
archive_command = 'pgbackrest --stanza=data archive-push %p'
archive_mode = on
```

```ini
# pgbackrest.conf
[data]
pg1-path = {{.DataDirectory}}

[global]
repo1-path = /var/lib/pgbackrest
repo1-retention-full = {{.Retention}}
```


## Remote Repository

```ini
# pgbackrest.conf (archive)
[data]
pg1-host = pgcontd.localhost
```
```ini
# pgbackrest.conf (primary / standby)
[global]
repo1-host = pgcontd.localhost
```

### Archive (Archive Push)
```graphviz
digraph {
 layout=dot rankdir=LR
 graph [style=rounded]
 node [shape=box]

 subgraph cluster1 { label="primary" pgcontd1 postgres1 -> pgbackrest1 }
 subgraph cluster2 { label="archive" pgcontd2 -> pgbackrest2 }

 # SSH
 pgbackrest1 -> pgcontd1 -> pgcontd2 [style=dashed]
}
```

### Backup
```graphviz
digraph {
 layout=dot rankdir=LR
 graph [style=rounded]
 node [shape=box]

 subgraph cluster1 { label="primary" pgcontd1 -> pgbackrest1 -> postgres1 }
 subgraph cluster2 { label="archive" pgcontd2 pgbackrest2 }

 # SSH
 pgbackrest2 -> pgcontd2 -> pgcontd1 [style=dashed]
}
```

### Restore (Archive Get)
```graphviz
digraph {
 layout=dot rankdir=LR
 graph [style=rounded]
 node [shape=box]

 subgraph cluster1 { label="standby / new primary" pgcontd1 postgres1 -> pgbackrest1 }
 subgraph cluster2 { label="archive" pgcontd2 -> pgbackrest2 }

 # SSH
 pgbackrest1 -> pgcontd1 -> pgcontd2 [style=dashed]
}
```

### Backup from Standby

https://pgbackrest.org/user-guide.html#standby-backup

```ini
# pgbackrest.conf (archive)
[data]
pg1-host = pgcontd.localhost
pg2-host = pgcontd.localhost
pg2-host-user = standby
pg2-path = {{.DataDirectory}}

[global]
backup-standby = y
```



Console Logging
```ini
# pgbackrest.conf
[global]
log-level-console = info
log-level-file = off
log-level-stderr = off
log-timestamp = n
```
