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
# Installation Fairyring
```
cd $HOME
rm -rf fairyring
git clone https://github.com/Fairblock/fairyring.git
cd fairyring
```
# Checkout to the latest release of fairyring (currently v0.6.0):
```
git checkout v0.5.0
```
# Install fairyringd:
```
make install
```
# Checkout Version:
```
fairyringd version
```
The output should show 0.6.0
check latest version : https://github.com/Fairblock/fairyring/releases
# Initialize the fairyring node:
* replace the NODE_NAME with a name of your choosing
```
fairyringd init NODE_NAME
```
* If you are unable to get the genesis file from the link above, command this:
```
cp ~/fairyring/networks/testnets/fairyring-testnet-1/genesis.json ~/.fairyring/config/genesis.json
```
# Set peers:
```
SEEDS=$(cat $HOME/fairyring/networks/testnets/fairyring-testnet-1/peers-node.txt)
echo $SEEDS
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$SEEDS\"/" $HOME/.fairyring/config/config.toml
```
# Start the node:
```
fairyringd start
```
# Validating on Fairblock Testnet:
# Create Wallet:
Replace WALLET_NAME with a name of your choosing
```
fairyringd keys add WALLET_NAME
```
# For Faucet: https://testnet-faucet.fairblock.network

* Ensure you write down the mnemonic as you can not recover the wallet without it.
* To ensure the wallet you created was saved to your node:
```
 fairyringd keys list
```
# check your balance: Replace WALLET_NAME OR ADDRESS with the wallet address you just created
```
fairyringd q bank balances ADDRESS
```
# command for creating a validator:
* Replace [ACCOUNT_KEY_NAME] with WALLET_NAME & [validator_moniker] with YOUR_NAME node.
```
fairyringd tx staking create-validator \
  --amount `10000000000ufairy` \
  --commission-max-change-rate 0.01 \
  --commission-max-rate 0.2 \
  --commission-rate 0.1 \
  --from [ACCOUNT_KEY_NAME] \
  --min-self-delegation 1 \
  --moniker [validator_moniker] \
  --security-contact "[optional-security@example.com]" \
  --website [optional.web.page.com] \
  --details [optional-details] \
  --pubkey $(fairyringd tendermint show-validator) \
  --chain-id fairyring-testnet-1
  ```
# Create a genesis transaction to become validator:
* Replace [ACCOUNT_KEY_NAME] with WALLET_NAME & [validator_moniker] with YOUR_NAME node.
```
fairyringd gentx \
  [account_key_name] \
  10000000000ufairy \
  --commission-max-change-rate 0.01 \
  --commission-max-rate 0.2 \
  --commission-rate 0.05 \
  --min-self-delegation 1 \
  --details [optional-details] \
  --identity [optional-identity] \
  --security-contact "[optional-security@example.com]" \
  --website [optional.web.page.com] \
  --moniker [node_moniker] \
  --chain-id fairyring-testnet-1
```
# After running the command above, it will create a gentx-XXXXXX.json file under this directory:

$HOME/.fairyring/config/gentx/gentx-XXXXXX.json

# Copy the contents genesis transaction (Your own specific genesis transaction directory) to fairyring/gentxs/ directory (replace VALIDATOR_NAME to your validator name = moniker):
```
cp $HOME/.fairyring/config/gentx/gentx-*.json $HOME/fairyring/networks/testnets/fairyring-testnet-1/gentxs/gentx-VALIDATOR_NAME.json
```
# Create a json file with all your node information:
# Do this:
```
touch peers-VALIDATOR_NAME.json
```
```
nano peers-VALIDATOR_NAME.json
```
# Copy following content to file .json:
```
{
  "node_id": "YOUR_NODE_ID",
  "public_ip": "YOUR_IP",
  "port": "YOUR_PORT"
}
```
get your node_id:
```
fairyringd tendermint show-node-id
```
get your public_ip:
```
curl ipinfo.io/ip
```
* your port, the default is 26656 if you did not change the config.
* After applying changes : C + X + Y  >>>> inter
# Next:
```
mv peers-VALIDATOR_NAME.json $HOME/fairyring/networks/testnets/fairyring-testnet-1/peers/
```
