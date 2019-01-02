---
title: "Backup to BackBlaze with Duplicity"
date: "2019-01-01"
image: images/katrin-leinfellner-571533-unsplash.jpg
customFooterPartial: Photo by Katrin Leinfellner on Unsplash
---
Simple backup solution for my (user) data.
<!--more-->

## Tools selection

To regularly backup my data I use [Duplicity][Duplicity] as a software and [BackBlaze][BackBlaze] as a cloud storage.

The main reasons for Duplicity:

- Support for various protocols
- Included in OpenSuse
- Stable and well documented

The main reasons for BackBlaze:

* Price
  * The price depends on number of files, access pattern etc. so it is not easy to estimate but my experience is that full backup of approx 100GB (photos, small documents etc.) costs around $6 and monthly fee (including ~100MB daily backup) is less than $2.
* Support in Duplicity
* Neat web UI

## Installation

```sh
zypper -n install duplicity python2-pip
pip2 install b2
```

## Backup command

I store the secrets (env. variables) in KeePassXC. `ACCOUNT_ID`, `APP_KEY` and `BUCKET` name can be obtained from backblace web user interface.

```
ACCOUNT_ID=
APP_KEY=
BUCKET=krabice-backup
DIR=nextcloud_data
PASSPHRASE=<gpg password>

duplicity list-current-files b2://${ACCOUNT_ID}:${APP_KEY}@${BUCKET}/${DIR}/
duplicity --allow-source-mismatch incremental /data/nextcloud/data/ b2://${ACCOUNT_ID}:${APP_KEY}@${BUCKET}/${DIR}/
```

Duplicity automatically selects incremental backup if there is a backup already. Otherwise it createsa full backup.


## Restoration commands

```
duplicity restore -r ${PATH_TO_RESTORE} b2://${ACCOUNT_ID}:${APP_KEY}@${BUCKET}/${DIR}/ ${LOCAL_DIR_TO_BE_CREATED}
```

---
Photo by Katrin Leinfellner on Unsplash

[Duplicity]: http://duplicity.nongnu.org/
[BackBlaze]: https://www.backblaze.com/
[HowTo]: https://www.backblaze.com/blog/backing-linux-backblaze-b2-duplicity-restic/

