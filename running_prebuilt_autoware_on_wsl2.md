# Windows で WSL を使って Autoware をシミュレートする

## Windows Subsystem for Linux (WSL) のセットアップ

Windows では仮想化技術を用いて Windows 内で Linux イメージを実行させることができます。この手順では WSL2 をインストールし、Ubuntu と呼ばれる OS を動作させます。

WSL のインストールの詳細	については、Microsoft の公式ドキュメントが役に立ちます: https://learn.microsoft.com/ja-jp/windows/wsl/install

### WSL2 を新規にインストールする

1. PowerShell を管理者権限で開き、`wsl --install` と入力し実行します。
2. PC を再起動するよう言われるので、再起動します。
3. 再起動後、WSL 用のユーザー名とパスワードを決めて入力します。
4. 少し待つと Ubuntu が起動します。

> :information_source: ユーザー名とパスワードは任意のものを使用して構いませんが、パスワードは忘れないようにしましょう。

> :information_source: パスワード入力時、何も表示されなくても焦らないでください！セキュリティのため、入力した文字列は表示されません。

### すでに WSL がインストールされているなら

もしすでに WSL がインストールされているのであれば、PowerShell を管理者権限で開き、`wsl --update` を実行して最新の WSL2 にアップデートします。
自分用の WSL の環境がすでに整っており、Autoware のインストールにより環境を破壊したくない場合は `wsl --install -d Ubuntu-22.04` などで別のディストリビューションをインストールするのも良いでしょう。

## Ubuntu の準備

インストールされた Ubuntu はコマンドでの操作を基本とします。
慣れないうちは、コマンドをコピーペーストして実行するのが良いでしょう（WSL の場合は右クリックで貼り付けができます）。

### パッケージをアップデートする

インストールしたばかりの Ubuntu に入っているパッケージは古いため、すべてのパッケージをアップデートします。
コマンド実行時にはパスワードを求められるので入力します。

```bash
sudo apt update
sudo apt full-upgrade -y
sudo apt autoremove -y
```

> :information_source: Ubuntu に限らず、アップデートを行うことは非常に大切なことです。こまめにアップデートをしましょう。

## Autoware の実行に必要な依存関係のセットアップ

### Docker

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG docker $USER
```

### rocker

```bash
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
sudo apt update
sudo apt install -y python3-rocker
```

### 変更を適用するために再ログインが必要

`exit` などを実行して終了してから、Ubuntu を起動しなおします。

## Autoware をシミュレーションをする

### シミュレーションに必要なイメージやデータを入手する（初回のみ）

```bash
docker pull ghcr.io/autowarefoundation/autoware-universe:humble-latest-prebuilt

sudo apt install -y unzip python3-pip && sudo pip3 install gdown
mkdir ~/autoware_map && cd ~/autoware_map
gdown 'https://docs.google.com/uc?export=download&id=1499_nsbUbIeturZaDj7jhUownh5fvXHd'
unzip sample-map-planning.zip
```

### docker の環境に入る

Autoware を実行するために、先ほどプルしたイメージを使用します。

```bash
rocker --x11 --user --volume $HOME/autoware_map -- ghcr.io/autowarefoundation/autoware-universe:humble-latest-prebuilt
```

### プランニングシミュレーターを起動する

操作方法やどのようなことができるのかは、Autoware の公式チュートリアルを参考にしてください: https://autowarefoundation.github.io/autoware-documentation/main/tutorials/ad-hoc-simulation/planning-simulation/#basic-simulations

```bash
ros2 launch autoware_launch planning_simulator.launch.xml map_path:=$HOME/autoware_map/sample-map-planning vehicle_model:=sample_vehicle sensor_model:=sample_sensor_kit
```
