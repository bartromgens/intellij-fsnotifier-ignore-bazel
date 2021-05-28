# IntelliJ fsNotifier fix for node_modules .staging

fsNotifier chokes on some code bases that link onto themselves because it uses `inotify` watch descriptors as file system node indices. These are unique per inode but sometimes inodes are linked or moved. This is a table collision (the watch descriptors match but the paths don't) and so fsNotifier bails out. If it bails out too often, IntelliJ stops watching the file system.

On the codebase I work with, this was exclusively happening to my project directories when `npm` was downloading to `.staging` directories and re-linking after. Our process refreshes certain modules on every build, so a few builds quickly stopped IntelliJ from watching the file system.

My hacky fix is to ignore paths that contains `.staging`: https://github.com/nuzayets/intellij-fsNotifier-hack/blob/master/inotify.c#L236

Use like this:
```bash
./make.sh
sudo mv fsnotifier64 /opt/JetBrains/idea-IU-211.7142.45/bin/fsnotifier64
```
