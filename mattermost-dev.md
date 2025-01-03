# Mattermostの開発環境構築

## 環境

WSL2 Ubuntu 22.04 LTS

## 起動までにやったこと

[https://developers.mattermost.com/contribute/developer-setup/](https://developers.mattermost.com/contribute/developer-setup/)

Docker上でサーバとwebappを起動する

### 前準備

更新

```
sudo apt update
```
```
sudo apt upgrade
```

### makeのインストール

```
sudo apt install build-essential
```

### Docker Engineのインストール

[https://docs.docker.com/engine/](https://docs.docker.com/engine/)

以前は以下のような名前でパッケージをリリースしていたが、名前が変わっているのでアンインストールすること

* docker.io
* docker-compose
* docker-compose-v2
* docker-doc
* podman-docker

```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

Dockerを初めてインストールする場合は以下のセットアップ必須

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Dockerのインストール

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

動作確認

```
sudo docker run hello-world
```

### Docker Composeのインストール

開発では使用しないが、一応インストール

```
sudo rm /usr/local/bin/docker-compose
sudo ln -s /Applications/Docker.app/Contents/Resources/cli-plugins/docker-compose /usr/local/bin/docker-compose
```

### Goのインストール

[https://go.dev/doc/install](https://go.dev/doc/install)

既存のGoを削除して追加

```
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.23.4.linux-amd64.tar.gz
```

.profileに以下を追加

```
# go
export GO_HOME=/usr/local/go
export PATH=$PATH:$GO_HOME/bin
```

### nvmのインストール

[https://learn.microsoft.com/ja-jp/windows/dev-environment/javascript/nodejs-on-wsl](https://learn.microsoft.com/ja-jp/windows/dev-environment/javascript/nodejs-on-wsl)

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
```

最新版のNode安定版をインストール

```
nvm install --lts
```

### bashの設定

.bashrcに以下を追加

```
# Increase the number of available file descriptors.
ulimit -n 8096
```

### libpngのインストール

### リポジトリの準備

forkしてcloneする

### サーバの起動

```
cd server
make run-server
```

### webappの起動

```
cd webapp
make run
```
