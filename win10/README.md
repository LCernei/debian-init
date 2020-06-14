# Windows 10 configuration

## WSL git

I use Windows 10 WSL (debian). To confire IDEs to use the WSL git comand:\
Create a `git.cmd` file in a separate location (`C:\Users\user\source\wslWrapper`).\
Add this location to system Path.

Write the following lines inside `git.cmd`:
```bat
@ECHO OFF
%WINDIR%\System32\bash.exe -c "git %*"
```

When the IDE looks for `git.exe` indicate `C:\Users\user\source\wslWrapper\git.cmd`.

[Tip Source](https://intellij-support.jetbrains.com/hc/en-us/community/posts/115000176290/comments/360000277759)
