# IntelliJ fsNotifier ignore bazel

Even if directories are excluded in IntelliJ IDEs, they are not ignored in fsnotifier. 
Issue for this problem: https://youtrack.jetbrains.com/issue/IDEA-73309/When-a-folder-is-excluded-from-the-project-is-still-watched-with-inotify

On the codebase I work with, this was a problem with huge bazel directories that a symlinked in the project (this is how bazel works).

My hacky fix is to ignore paths that contains `bazel-`.

## Build
Install peer dependencies:
```bash
apt install build-essential libc6-dev-i386 libgcc-9-dev-i386-cross
```

Compile with this command:
```bash
./make.sh
```

## Use
Then replace the existing notifier:

```bash
sudo mv fsnotifier <jetbrains-ide-dir>/bin/fsnotifier
```
Or
```bash
sudo mv fsnotifier64 <jetbrains-ide-dir>/bin/fsnotifier64
```
