## 软件安装

#### VSCode安装

```shell
# 安装vscode
sudo apt update
sudo apt install software-properties-common apt-transport-https curl

curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"

sudo apt update
sudo apt install code

# 更新vscode
sudo apt update
sudo apt upgrade
```

