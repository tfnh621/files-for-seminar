# なぜ Windows だけではだめなのか
- ROS2 のインストールは結構簡単
- しかし `rmw_cyclonedds_cpp` が Windows に対応していなかった…

# WSL のインストール
1. PowerShell を管理者で開き、`wsl --install` を実行する
2. PC を再起動するよう言われるので、再起動する
3. 再起動後、WSL 用のユーザー名とパスワードを決めて入力する
4. 完了

# ROS2 をぶっこむ
ココ参考にしたよ: https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html

## Ubuntu のアップデート
```bash
sudo apt update && sudo apt full-upgrade -y && sudo apt auto-remove -y
```

## インストールるよ
```bash
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```
```bash
sudo apt update && sudo apt install ros-humble-ros-base ros-dev-tools -y
```
```bash
sudo apt install ros-humble-rmw-cyclonedds-cpp -y
```

# 入門やってみよう
```bash
#export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
#export CYCLONEDDS_URI=file://$PWD/cyclonedds.xml
source /opt/ros/humble/setup.bash
```
