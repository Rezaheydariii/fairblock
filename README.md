# Fairblock Node validator 
![Capture](https://github.com/Rezaheydariii/fairblock/assets/140112620/1ee72e5c-ec2b-4160-8086-ab53a812b41e)

# Install dependencies:
```
sudo apt update && sudo apt upgrade -y
sudo apt install git curl tar wget libssl-dev jq build-essential gcc make
```
# Download and install Go:
```
sudo add-apt-repository ppa:longsleep/golang-backports
```
```
cd $HOME && \
ver="1.21" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```
