---
title: "TIL: journalctl!"
date: 2023-04-30T11:30:03+06:00
showToc: true
TocOpen: false
draft: false
tags: ["TIL"]
---

`jounralctl` is the command line tool that let us interact with the journal logs. Default location of `journald` logs is `/var/log/journal` directory.

Type `journalctl` in the terminal, it will show the journal logs in chronological order.

- `journalctl --no-pager` will display entire logs directly on the screen.
- `journalctl -n 25`: will display most recent 25 lines of the logs.
- `journalctl -f`:  view logs in real time.
- `jouralctl --utc`: display  logs in UTC time.
- `journalctl -k`: display only  kernel logs.

#### All Journal logs

Use `sudo` to see all journal logs.

```bash
sudo journalctl -u ssh

journalctl -u service_name
```

One can also filter journal logs for a specific systemd service by thins way.

#### Show messages from a particular boot session

The `journalctl` command allows to access logs belonging to a specific boot session using the option `-b`.

List all the boot sessions with `--list-boots` flag.

```bash
journalctl --list-boots
```

### Filter logs

Use natural language to filter logs:

```bash
journalctl --since=yesterday --until=now
```

Terms like *yesterday, tomorrow* and *today* are recognized.

Use date or date time combination:

```bash
journalctl --since "2023-03-10"
```

Specify a time period with date and time:

```bash
journalctl --since "2023-04-10 15:10:00" --until "2023-07-10"
```

Journal logs can also be filtered on **UID** (*User ID*), **GID** (*Group ID*) and **PID** (*Process ID*).

```bash
journalctl _PID=1234
```

# Tips

Combine more than one options for more tailored log viewing. For example, to see ssh logs from yesterday in UTC timestamps:

```bash
sudo journalctl -u ssh --since=yesterday --utc
```

See ssh logs in the current session from last boot:

```bash
sudo journalctl -u ssh -b0
```

To view last few logs: `journalctl -xe`.

- `-e`: jump to the end of the journal logs.
- `-x`: shows extra info on the log entries.

#### Show only errors in logs with journalctl

To show all the errors in the current session:

```bash
journalctl -p 3 -xb
```

- `-p 3`: filter logs for priority 3 (*which is error*)
- `-x`: provides additional information on the log (*if available*).
- `b`: since last boot (*which is the current session*).

Use other priority level to get debug or warning or even critical level logs:
 | Priority | Code |
 |:--------:|------|
 | 0 | emerg |
 | 1 | alert |
 | 2 | crit |
 | 3 | err |
 | 4 | warning |
 | 5 | notice |
 | 6 | info |
 | 7 | debug |

 See all the warning, notice and info logs from the current session: `journalctl -p 4..6 -b0`

 `journalctl --disk-usage` will show how much disk space the journal logs are taking with this journal command.

### `timedatectl`

- `timedatectl`
- `timedatectl list-timezones` : see available timezone.
- `timedatectl set-timezone` : set time.

### `journalctl`

To see the logs that the `journald` daemon has collected, use `journalctl`.

- `journalctl -b`: shows entries that collected from the most recent boot.

By making persistent storage option, we can list boots from different types. Changing configuration under `/etc/systemd/journald.conf`:

```bash
[Journal]
Storage=persistent
```

- `journalctl --list-boots`: To see the boots `journald` knows about.

### Time windows

- `--since`:
- `--until`:
