# IntelliJ fsNotifier ignore bazel

fsNotifier chokes on some code bases that link onto themselves because it uses `inotify` watch descriptors as file system node indices. 
These are unique per inode but sometimes inodes are linked or moved. 
This is a table collision (the watch descriptors match but the paths don't) and so fsNotifier bails out. 
If it bails out too often, IntelliJ stops watching the file system.

Even if files are excluded, they are not ignored in fsnotifier. Issue for this problem: https://youtrack.jetbrains.com/issue/IDEA-73309/When-a-folder-is-excluded-from-the-project-is-still-watched-with-inotify

On the codebase I work with, this was exclusively happening to my project directories when `npm` was downloading to `.staging` directories and re-linking after. 
Our process refreshes certain modules on every build, so a few builds quickly stopped IntelliJ from watching the file system.

My hacky fix is to ignore paths that contains `bazel-`: https://github.com/nuzayets/intellij-fsNotifier-hack/blob/master/inotify.c#L236

# Build
Install peer dependencies:
```bash
apt install build-essential libc6-dev-i386 libgcc-9-dev-i386-cross
```

Compile with this command:
```bash
./make.sh
```

Then replace the existing notifier:

```bash
sudo mv fsnotifier <jetbrains-ide-dir>/bin/fsnotifier
```
Or
```bash
sudo mv fsnotifier64 <jetbrains-ide-dir>/bin/fsnotifier64
```
