<h2 align=center>Mint first CAT20 token CAT on Fractal Mainnet</h2>

- Install Docker
```bash
 apt update &&  apt install -y curl && curl -fsSL https://get.docker.com -o get-docker.sh &&  sh get-docker.sh
```
- Install Docker Compose
```bash
 curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
- Make this executable
```bash
 chmod +x /usr/local/bin/docker-compose
```
- Install `npm`
```bash
 apt-get install npm -y
```
- Install `n` pacakges globally
```bash
 npm i n -g
```
- Switch to stable node version
```bash
 n stable
```
- Install `yarn` package globally
```bash
 npm i -g yarn
```
- Clone `Cat Protocol` repo
```bash
git clone https://github.com/CATProtocol/cat-token-box && cd cat-token-box
```
```bash
 chown -R $USER:$USER ~/cat-token-box
```
- Install and build this project
```bash
 yarn install && yarn build
```
- Change directory to `tracker`
```bash
cd packages/tracker
```
- Run `Fractal Bitcoin` node
```bash
 chmod 777 docker/data &&  chmod 777 docker/pgdata &&  docker compose up -d
```
- Build docker image under the project root directory
```bash
cd $HOME/cat-token-box &&  docker build -t tracker:latest .
```
- Run the container
```bash
 docker run -d \
    --name tracker \
    --add-host="host.docker.internal:host-gateway" \
    -e DATABASE_HOST="host.docker.internal" \
    -e RPC_HOST="host.docker.internal" \
    -p 3000:3000 \
    tracker:latest
```
- Change directory
```bash
cd $HOME/cat-token-box/packages/cli
```
```bash
cat <<EOF > config.json
{
  "network": "fractal-mainnet",
  "tracker": "http://127.0.0.1:3000",
  "dataDir": ".",
  "maxFeeRate": 30,
  "rpc": {
      "url": "http://127.0.0.1:8332",
      "username": "bitcoin",
      "password": "opcatAwesome"
  }
}
EOF
```
---
- Use below command to create a new wallet address
```bash
 yarn cli wallet create
```
<h2 align=center> Or </h2>

**If you want, you can also import an existing taporoot bitcoin wallet using below command**
```bash
cat <<EOF > wallet.json
{
  "accountPath": "m/86'/0'/0'/0/0",
  "name": "cat-5d0fe77c",
  "mnemonic": "YOUR TAPROOT ADDRESS MNEMONIC PHRASE"
}
EOF
```
---
- You can see your wallet address using this command
```bash
 yarn cli wallet address
```
- Now u need to have $FB in order to mint CAT token, so to get $FB , first import these seed phrase in [Unisat Wallet](https://chrome.google.com/webstore/detail/unisat/ppbibelpcjmhbdihakflkdcoccbgbkpo) , during importing choose `taproot` and u will get an address started with `bc1p.......`
- Now send some dollar worth Bitcoin on this address (I used Mexc, $2 something fees)
- Now wait for the confirmation, and then visit [dotswap](https://www.dotswap.app/v1/swap#F_BTC_FB) and deposit BTC to this platform and then swap from BTC to FB using this platform, after swapping withdraw FB from this platfotm to your wallet address
- Now wait until your tracker get synchronised with latest block, u can use this command to check sync status
```bash
 yarn cli wallet balances
```
- You will see something like this, if it is 100%, after that u can mint CAT token

![image](https://github.com/user-attachments/assets/4abfd1d1-b1fb-461c-89a4-7788db9c88c1)

- Use this command to install `curl` and `jq`
```bash
dpkg -l | grep -q '^ii  curl ' ||  apt-get install -y curl && dpkg -l | grep -q '^ii  jq ' ||  apt-get install -y jq
```
- Use this command to mint with current market fee
```bash
 yarn cli mint -i 45ee725c2c5993b3e4d308842d87e973bf1951f5f7a804b21e4dd964ecd12d6b_0 5 --fee-rate $(curl -s https://explorer.unisat.io/fractal-mainnet/api/bitcoin-info/fee | jq '.data.fastestFee')
```
- You can check balance using this command
```bash
 yarn cli wallet balances
```
- Done ✅
