# インストール

https://ms-iot.github.io/ROSOnWindows/GettingStarted/SetupRos2.html を参考にしたよ

1. git 必要かも
2. chocolaty も必要かも
3. Visual Studio Community 2022 を入れる
4. `C++ によるデスクトップ開発` を入れる
5. `x64 Native Tools Command Prompt for VS 2022` を管理者権限で開く
6. 実行:
```bat
mkdir C:\opt\chocolatey
set ChocolateyInstall=C:\opt\chocolatey
choco source add -n=ros-win -s="https://aka.ms/ros/public" --priority=1
choco upgrade ros-humble-desktop -y --execution-timeout=0 --pre
```

# tutorial
`x64 Native Tools Command Prompt for VS 2022` を普通に開く
```
C:\opt\ros\humble\x64\setup.bat
```
これで `ros2` コマンドが使えるようになる

```bash
ros2 topic pub /topic std_msgs/String "data: Hello"
```
