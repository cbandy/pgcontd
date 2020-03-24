# Streaming replication

https://www.postgresql.org/docs/current/warm-standby.html#STREAMING-REPLICATION

| Parameter | | |
|-----------|-|-|
| `max_connections` | S | `100`, GTE primary
| `max_locks_per_transaction` | S | `64`, GTE primary
| `max_prepared_transactions` | S | `0`, GTE primary
| `max_replication_slots` | S | `10`, GTE the existing slots
| `max_wal_senders` | S | `10`, GT zero, GTE primary
| `max_worker_processes` | S | `8`, GTE primary
| `track_commit_timestamp` | CF | `off`, match primary
| `wal_level` | S | `replica`, or `logical`
| `wal_keep_segments` | CF | `0`
| -- on primary --
| `synchronous_standby_names` | CF | (off)
| -- on replica --
| `hot_standby` | S | `on`
| `primary_conninfo` | S |
| `primary_slot_name` | S |
| `promote_trigger_file` | CF |
| `recovery_min_apply_delay` | CF | affects synchronous commit
| `recovery_target_timeline` | S | `latest`

```
(data_directory)/standby.signal
```

- Ensure streaming replication is enabled.
- Ensure [WAL](WAL.md) segments are available to replicas.
- Ensure replicas can [authenticate](authentication.md) with primary.
- Ensure replicas are authorized on primary.
- Manage (add/remove) replication slots?

## Hot standby

- Allow for replicas outside of HA.
- Allow for delayed replicas outside of HA.
- Allow for `pg_receivewal`, both in and out of HA.
