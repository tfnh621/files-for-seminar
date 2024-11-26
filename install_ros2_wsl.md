# なぜ Windows だけではだめなのか
- ROS2 のインストールは結構簡単
- しかし `rmw_cyclonedds_cpp` が Windows に対応していなかった…

# WSL のインストール
1. PowerShell を管理者で開き、`wsl --install -d Ubuntu-22.04` を実行する
2. PC を再起動するよう言われるので、再起動する
3. 再起動後、WSL 用のユーザー名とパスワードを決めて入力する
4. 完了

# ROS2 をぶっこむ
ここを参照してください：
https://bookstack.saikocar.jp/books/beed1/page/ros-2-humble-ubuntu
