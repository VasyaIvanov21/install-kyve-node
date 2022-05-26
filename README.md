# install-kyve-node

Примерные требования к серверу

2 CPU  4 GB RAM  60 GB SSD - Ubuntu 20.04

Для начале нужно получить небольшоое количество токенов AR. Их можно купить на бирже или получить через бесплатный  [кран](https://faucet.arweave.net/). Для этого понадобится твиттер аккаунт. Перейдите на  [сайт](https://faucet.arweave.net/), установите галки и скачайте json файл. Затем переименуйте его в "arweave.json". Далее нажмите на "Open tweet pop-up", опубликуйте твит и через некоторое время вам придет небольше количество токенов AR. Затем перенесите файл arweave.json к себе на сервер с помощью scp или filezilla в домашнюю папку вашего пользователся.


Установка оркужения:
```
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt install unzip -y

```
 
Созданйте папку kyve в домашней директории
```
mkdir $HOME/kyve
cd $HOME/kyve

```
Затем скачивайте бинарные файлы ( с добавлением новых пулов возможно будут появляться новые бинарники )

```
wget -O evm-linux.zip https://github.com/KYVENetwork/evm/releases/download/v1.0.5/evm-linux.zip
wget -O kyve-bitcoin-linux.zip https://github.com/kyve-org/bitcoin/releases/download/v0.0.0/kyve-bitcoin-linux.zip
wget -O kyve-solana-linux.zip https://github.com/kyve-org/solana/releases/download/v0.0.1/kyve-solana-linux.zip
wget -O kyve-zilliqa-linux.zip https://github.com/kyve-org/zilliqa/releases/download/v0.0.0/kyve-zilliqa-linux.zip
wget -O stacks-linux.zip https://github.com/kyve-org/stacks/releases/download/v0.0.2/stacks-linux.zip
wget -O celo-linux.zip https://github.com/kyve-org/celo/releases/download/v0.0.0/kyve-celo-linux.zip
wget -O near-linux.zip https://github.com/kyve-org/near/releases/download/v0.0.1/kyve-near-linux.zip
wget -O kyve-evmos-linux.zip https://github.com/kyve-org/evm/releases/download/v1.0.5/kyve-evm-linux.zip
wget -O kyve-cosmos-linux https://github.com/kyve-org/cosmos/releases/download/v0.0.0/kyve-cosmos-linux.zip

```
Распаковуйте архивы, сделайте файлы исполняемыми и перенесите их в папку /usr/local/bin/

```
unzip -o "*.zip"
chmod +x evm-linux kyve-solana-linux kyve-zilliqa-linux bitcoin-linux stacks-linux kyve-celo-linux  kyve-near-linux kyve-evm-linux kyve-cosmos-linux
mv kyve-cosmos-linux evm-linux kyve-solana-linux kyve-zilliqa-linux bitcoin-linux stacks-linux kyve-near-linux kyve-evm-linux  kyve-celo-linux /usr/local/bin/

```
Затем создайте сервисные файлы для каждого пула
```
MNEMONIC="указываем вашу сид фразу (мнемонику )"
```
```
STAKE="указываем количество токенов, которое вы хотите застейкать"
```
пул Moonbeam
```
echo "[Unit]
Description=Moonbeam
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which evm-linux) --poolId 0 --mnemonic \"$MNEMONIC\" --keyfile $HOME/arweave.json --initialStake $STAKE
Restart=on-failure
LimitNOFILE=65535
unzip -o "*.zip"
[Install]
WantedBy=multi-user.target" > $HOME/kyve/service/moonbeam.service

```

пул Avalanche

```
echo "[Unit]
Description=Avalanche
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which evm-linux) --poolId 1 --mnemonic \"$MNEMONIC\" --keyfile $HOME/arweave.json --initialStake $STAKE
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/kyve/service/avalanche.service


```
пул Bitcoin

```
echo "[Unit]
Description=Bitcoin
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which bitcoin-linux) --poolId 3 --mnemonic \"$MNEMONIC\" --keyfile $HOME/arweave.json --initialStake $STAKE
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/kyve/service/bitcoin.service

```

пул Solana

```
echo "[Unit]
Description=Solana
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which kyve-solana-linux) --poolId 4 --mnemonic \"$MNEMONIC\" --keyfile $HOME/arweave.json --initialStake $STAKE
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/kyve/service/solana.service

```

пул Zilliqa

```
[Service]
User=$USER
Type=simple
ExecStart=$(which kyve-zilliqa-linux) --poolId 5 --mnemonic \"$MNEMONIC\" --keyfile $HOME/arweave.json --initialStake $STAKE
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/kyve/service/zilliqa.service

```

пул Near
```
echo "[Unit]
Description=near
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which kyve-near-linux) --poolId 6 --mnemonic \"$MNEMONIC\" --keyfile $HOME/arweave.json --initialStake $STAKE
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/kyve/service/near.service

```

пул Celo

```
echo "[Unit]
Description=celo
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which kyve-celo-linux) --poolId 7 --mnemonic \"$MNEMONIC\" --keyfile $HOME/arweave.json --initialStake $STAKE
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/kyve/service/celo.service

```

пул Evmos
```
echo "[Unit]
Description=evmos
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which evm-linux) --poolId 8 --mnemonic \"$MNEMONIC\" --keyfile $HOME/arweave.json --initialStake $STAKE
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/kyve/service/evmos.service

```
пул Cosmos

```
echo "[Unit]
Description=cosmos
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which kyve-cosmos-linux) --poolId 9 --mnemonic \"$MNEMONIC\" --keyfile $HOME/arweave.json --initialStake $STAKE
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/kyve/service/cosmos.service

```

Перенесите сервисные файлы в папку /etc/systemd/system/
```
mv $HOME/kyve/service/* /etc/systemd/system/
sudo tee <<EOF >/dev/null /etc/systemd/journald.conf
Storage=persistent
EOF
sudo systemctl daemon-reload
sudo systemctl restart systemd-journald
```

Запуск пула ( при необходимости с новым значение токенов для стейка ) на примере пула Cosmos

```
STAKE=NEW_STAKE
sed -i.bak -e "s/initialStake .*/initialStake $STAKE/" /etc/systemd/system/cosmos.service
systemctl daemon-reload
systemctl restart cosmos

```
просмотр логов

```
journalctl -u cosmos -f

```
Стоить помнить, что минимальное значение количества токенов для бонда в пулах постоянно меняется. В каждом пуле активный сет равен 50 валидаторов. Если вы выпадаете из этого числа, то ваши застейканные тонкены автоматически расстейкиваются.

