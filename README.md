# IntelliJ fsnotifier ignore bazel

IntelliJ's fsnotifier does not ignore directories excluded in IntelliJ IDEs.
Issue for this problem: https://youtrack.jetbrains.com/issue/IDEA-73309/When-a-folder-is-excluded-from-the-project-is-still-watched-with-inotify

This can be a problem with huge directories inside the project.
One example are large bazel symlinked directories in the project (this is how bazel works).

The hacky fix implmented in this version of fsnotifier is to ignore paths that contains `bazel-`.

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
