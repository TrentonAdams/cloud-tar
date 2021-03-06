### 2.4.0
- support tar --no-check-device so that in cases where device numbers have
  changed we don't essentially do a level 0 as if it were an incremental backup.
  This is useful in the case of a restore to a new computer, followed by an
  incremental backup of said computer.  Also, wsl in Windows 10 seems to change
  device numbers or something.

### 2.3.3
- make gpg file detection more reliable.  Specifically the previous method
  didn't work unless the gpg key was already cached

### 2.3.2
- fix gpg decryption bug

### 2.3.1
- README update for 2.3.0

### 2.3.0
- add support for restoring backups by name from a source folder (the backup folder)

### 2.2.0
- support multiple gpg recipients with multiple -r arguments.
- use milliseconds since 1970 as backup index.

### 2.1.0
- Add --sub-folder option; see `cloud-tar -h` for more information

### 2.0.2
- More restructuring

### 2.0.1
- Restructured code into functions

### 2.0.0
- Add tests for most things

### 1.0.1
- Minor cleanup stuff

### 1.0.0
- Initial release with basic working features