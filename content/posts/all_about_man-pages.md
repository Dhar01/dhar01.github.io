---
title: "TIL: man pages!"
date: 2022-10-13T11:30:03+06:00
showToc: true
TocOpen: false
draft: false
tags: ["TIL"]
---

`man` - display a program's manual page.

```bash
man program # program is the name of the command
```

On most Linux systems, `man` uses `less` to display manual page.

The *manual* that man displays is broken into sections and covers not only user commands but also system administration commands, programming interfaces, file formats
and more.

Man page organization:
| Section | Contents |
|:-------:|----------|
| `1` | user commands |
| `2` | programming interfaces for kernel system calls |
| `3` | programming interfaces to the C library |
| `4` | special files such as *device nodes and drivers* |
| `5` | file formats |
| `6` | games and amusements such as *screen savers* |
| `7` | Miscellaneous |
| `8` | System administration commands |
| `9` | Obscure kernel specs and interfaces |

```bash
man section search_item

man 5 passwd
```

Refer to a specific section of the manual to find what we are looking for.

> `man -k` will perform the same as `apropos`.

If our system has additional man pages, we can rebuild this file with `makewhatis` (*Red Hat and FreeBSD*) or `mandb` (*Ubuntu*).

# Extra

Tools to use alongside man pages

- `apropos`
