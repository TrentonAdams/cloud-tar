# Cloud TAR
                   
WARNING: this project probably does not support MacOS/BSD unless you install the appropriate GNU utilities.  If you'd like it to work with default MacOS/BSD tools, please submit a patch that detects them and uses different behaviour for MacOS/BSD only; complete with tests.  It should detect the type of tool, not the platform.

Backup scripts to manage incremental backups using gnu tar's incremental snapshot capability, while supporting an s3 sync as well.

See [changelog.md](changelog.md)

## Install

Download the [latest release](https://github.com/TrentonAdams/cloud-tar/releases/latest) tar.gz.  If you just want to install directly to /usr/local/bin/, you can run the following...

```bash
# swap the -t for -x after you've confirmed the tar listing is only putting cloud-tar in `/usr/local/bin/`

curl -s https://api.github.com/repos/TrentonAdams/cloud-tar/releases/latest \
  | jq -r '.assets[0].browser_download_url' \
  | read -r latest; curl -s -L $latest \
  | sudo tar -tvz -C /usr/local/bin
```

Alternatively clone the repo and install in /usr/local/bin.

```bash
git clone --recurse-submodules git@github.com:TrentonAdams/cloud-tar.git
cd cloud-tar/
make clean tests install
```

## Usage                  
To back up...
1. user named "${USER}" from your USER environment var
2. to folder `/media/backup/cloud-tar`
3. with gpg encryption to gpg recipient `me@example.com`
4. an s3 sync to s3 bucket named user-backup-home
5. using `~/backup/tar-excludes.txt` as the tar exclusion file

So let's clone, run tests, and run a test backup.
    
```bash
cloud-tar backup \
  -s /home/${USER} \
  -d /media/backup/cloud-tar \
  -n home \
  -r me@example.com \
  -e ~/backup/tar-excludes.txt \
  -b user-backup-home;
```

An example tar-excludes.txt follows.  Replace `username` with your `${USER}`.  Tar exclude files differ from rsync exclude files.  With tar excludes you have to use the full path that you're backing up.  With rsync excludes, it's relative to the last folder in the path you're backing up.   

```text
/home/username/.config/google-chrome
/home/username/.cache
/home/username/.config/Code/Cache
/home/username/.config/Code/CacheData
```

## TODO

* add integrity check (`tar -tvzg file.sp`)
* add restore script.
* support syncing to sub-path in s3 bucket, so we can do repeated level 0 + successive backups.
* add backup deletion script, where we can delete success backups as far back as we need, and restore the snapshot file to that point.
  * Possibly `ls -1 backup-dir/name.*.spb | tail -2` to get previous backup snapshot file
  * Decrypt backup snapshot file to `backup-dir/name.sp`
  * Delete files for `ls -1 backup-dir/name.*.spb | tail -1`
  * Delete files for `ls -1 backup-dir/name.*.backup* | tail -1`
  * Previous won't quite work, as we need to account for split file names.  So we need to grab the timestamp from the most recent backup file name, and then deleted wildcarded `name.*.backup*`
* make "splitting" at 4G an option, not a requirement.
* create asciinema demo.
* add argument for backup size warning
