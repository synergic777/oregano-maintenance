# oregano-maintenance

## tiger-backup

requirements:
- macos:
```
# Update Homebrew
brew update

# Install rclone
brew install rclone
rclone version

# Install restic
brew install restic
restic version
```
- debian
```
sudo apt update
sudo apt install rclone -y; rclone version;
sudo apt update
sudo apt install restic -y
restic version
```
 
```
launchctl unload ~/Library/LaunchAgents/com.marcin.backup.plist;

launchctl load ~/Library/LaunchAgents/com.marcin.backup.plist; 

chmod +x backup.sh;
```

---

create new repo
```
restic -r rclone:gdrive:backups/serverOregano/restic init
```


do backup:
```
cd ~;
cd docker;
cd tiger-backup;
chmod +x backup.sh;
./backup.sh;
```

check media backups
```
cd ~;

cd docker;

cd tiger-backup;

 export RESTIC_REPOSITORY="rclone:gdrive:backups/serverOregano/restic";
 
 export RESTIC_PASSWORD_FILE="restic-password.txt";
 
 restic snapshots
```
 ---

launchctl start com.marcin.backup


---
tail -n 15 -F ~/Library/Logs/tiger-backup.log


---
kill -9 25455


---
restore
```
brew install rclone


; rclone config ; rclone ls gdrive: ;  export RESTIC_REPOSITORY="rclone:gdrive:path/to/restic-backup";
export RESTIC_PASSWORD="yourpassword";
restic snapshots;
restic restore latest --target ~/restored;
 


 / 
 
restic restore 8a006092 --target ~/music-09-20


or just

restic -r rclone:gdrive:backups/serverOregano/restic snapshots
restic -r rclone:gdrive:backups/serverOregano/restic restore <snapshotID> --target /restorePoint

```

---

 stop restoring task manager
ps aux | grep restic
kill -9 {ID}
