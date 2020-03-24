# Logging

https://www.postgresql.org/docs/current/runtime-config-logging.html

| Parameter | | |
|-----------|-|-|
| `log_destination` | CF | `stderr`, or `csvlog` or `syslog`
| `log_line_prefix` | CF | `%m [%p] `
| `logging_collector` | S | `off`, `on` to direct `stderr` to file(s)
| -- collector enabled --
| `log_directory` | CF | `log`, relative to `data_directory`
| `log_filename` | CF | `postgresql-%Y-%m-%d_%H%M%S.log`, strftime specification
| `log_file_mode` | CF | `0600`
| `log_rotation_age` | CF | `24h`, `0` to disable
| `log_rotation_size` | CF | `10MB`, `0` to disable
| `log_truncate_on_rotation` | CF | `off`
| -- syslog enabled --
| `syslog_facility` | CF | `LOCAL0`
| `syslog_ident` | CF | `postgres`
| `syslog_sequence_numbers` | CF | `on`, `off` to leave messages alone
| `syslog_split_messages` | CF | `on`, `off` to leave messages alone

- Allow logs from all processes to be collected to stdout and stderr.

When `logging_collector` is `on`, there will always be a `(log_directory)/(log_filename)`.
If `log_destination` lacks `stderr`, that file will contain:

```
(log_line_prefix)LOG:  ending log output to stderr
(log_line_prefix)HINT:  Future log output will go to log destination "(log_destination)".
```

## Pipe

```go
err := syscall.Mkfifo(path, 0600)
file, err := os.OpenFile(path, os.O_RDONLY, os.ModeNamedPipe) // blocks until path is opened for writing
io.Copy(out, file) // returns (file emits io.EOF) when writer closes path
```

## Scope creep

- Change the format of collected logs? (PostgreSQL CSV to JSON)
