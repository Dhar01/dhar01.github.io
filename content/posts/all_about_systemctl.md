---
title: "TIL: systemctl!"
date: 2023-03-20T11:30:03+06:00
showToc: true
TocOpen: false
draft: false
tags: ["TIL"]
---

# Service management

```bash
sudo systemctl start app.service

sudo systemctl stop app.service

sudo systemctl restart app.service

sudo systemctl reload app.service

sudo systemctl reload-or-restart app.service

sudo systemctl enable app.service

sudo systemctl disable app.service
```

### Checking status

```bash
systemctl status app.service

systemctl is-active app.service

systemctl is-enabled app.service

systemctl is-failed app.service
```

### System state overview

```bash
# see a list of active units
systemctl list-units

systemctl # returns same as above

systemctl list-units --all

systemctl list-units --all --state=inactive

systemctl list-units --type=service
```

To see every available units in the system paths (*including those that `systemd` has not attempted to load*):

```bash
systemctl list-unit-files
```

The state:

- `enabled`
- `disabled`
- `static`: means that the unit files does not contain an `install` section, which is used to enable a unit.
- `masked`:

# Unit management

### Show a unit file

To display the unit file that `systemd` has loaded into the system:

```bash
systemctl cat app.service
```

### Display dependencies

To see a unit's dependency tree:

```bash
systemctl list-depnedencies app.service
```

### Using shortcuts for important events

Put the system into rescue (*single-user*) mode:

```bash
sudo systemctl rescue
```

To halt the system:

```bash
sudo systemctl halt
```

To initiate a full shutdown:

```bash
sudo systemctl poweroff
```

For restart:

```bash
sudo systemctl reboot
```

### Listing available targets

A list of the available targets:

```bash
systemctl list-unit-files --type=target
```

Unlike runlevels, multiple targets can be active at one time. An active target indicates that `systemd` has attempted to start all of the units tied to the target and has not tried to tear them down again.

To see all of the active targets:

```bash
systemctl list-units --type=target
```

### Default target

To find the default target for system:

```bash
systemctl get-default
```

To set different default target:

```bash
sudo systemctl set-default graphical.target
```

### Editing unit files

Edit unit file:

```bash
sudo systemctl edit app.service
```

This will be a blank file that can be used to override or add directives to the unit definition. A directory will be created within the `/etc/systemd/system` directory which contains the name of the unit with .d appended. For instance, for the `nginx.service`, a directory called `nginx.service.d` will be created.

Within this directory, a snippet will be created called override.conf. When the unit is loaded, `systemd` will, in memory, merge the override snippet with the full unit file. The snippet’s directives will take precedence over those found in the original unit file.

To edit the full unit file instead of creating a snippet:

```bash
sudo systemctl edit --full nginx.service
```

After editing, deleting file or directory:

```bash
sudo systemctl daemon-reload
```

### Check unit properties

To see the low-level properties of a unit:

```bash
systemctl show app.service
```

To see the conflicts:

```bash
systemctl show app.service -p conflicts
```

### Masking or unmasking units

```bash
sudo systemctl mask app.service
```

To mark a unit as completely unstartable, automatically or manually, by linking it to /dev/null, we can mask the unit.

To unmask:

```bash
sudo systemctl unmask app.service
