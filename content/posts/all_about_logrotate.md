---
title: "TIL: logrotate!"
date: 2023-05-10T11:30:03+06:00
showToc: true
TocOpen: false
draft: false
tags: ["TIL"]
---

**Logrotate** is a system utility that manages the automatic rotation and compression of log files.

```bash
logrotate --version
man logrotate
```

Configuration can be found generally in two places:

- `/etc/logrotate.conf`: contain some default settings, sets up rotation for a few logs that are not owned by any system packages. Also uses an `include` statement to pull in configuration from any file from the logrotate directory.
- `/etc/logrotate.d`: files for packages.

By default, `logrotate.conf`will configure weekly log rotations, with log files owned by the root user and the **syslog** group., with four log files being retained at a time (*rotate 4*) and new empty log files being created after the current one is rotated (`create`).

Example:

```bash
/var/log/apt/term.log {
  rotate 12
  monthly
  compress
  missingok
  notifempty
}

/var/log/apt/history.log {
  rotate 12
  monthly
  compress
  missingok
  notifempty
}
```

Any settings in a logrotate file under `/etc/logrotate.d` will override logrotate's default values.

- `rotate 12`: keep twelve old log files.
- `monthly`: rotate once a month.
- `compress`: compress the rotated files. This uses `gzip` by default and results in files ending `.gz`. The compression command can be changed using `compresscmd` option.
- `missingok`: don't write an error message if the log file is missing.
- `notifempty`: don't rotate the log file if it is empty.

```bash
sudo logrotate /etc/logrotate.conf --debug
```

If we declare a logrotate file outside the default directory, we do need to specify a **state** file. The state file is stored in `/var/lib/logrotate/status` ; It keep records what `logrotate` found and if any actions it took the last time it ran.

```bash
logrotate /home/loki/logrotate.conf --state /home/loki/logrotate-state --verbose
```

If we want to force `logrotate` to rotate the the log file:

```bash
logrotate /home/loki/logrotate.conf --state /home/loki/logrotate-state --verbose
```

Finally, we need to set up a cron job to run Logrotate every hour:

```bash
crontab -e
```
