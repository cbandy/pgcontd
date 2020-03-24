# Write Ahead Log

https://www.postgresql.org/docs/current/wal-configuration.html

| Parameter | | |
|-----------|-|-|
| `checkpoint_completion_target` | CF |
| `checkpoint_timeout` | CF | time between checkpoints
| `max_wal_size` | CF | bytes between checkpoints
| `min_wal_size` | CF | bytes reserved for segments
| `wal_level` | S | `replica`, or `logical`
| `wal_keep_segments` | CF |
| -- archiving --
| `archive_command` | CF |
| `archive_mode` | S | `on`, behavior controlled with `archive_command`
| `archive_timeout` | CF |

## Scope creep

- Tune WAL archive/segments per RTO?
