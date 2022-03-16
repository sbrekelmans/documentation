# Troubleshooting FAQ

## 1. My public node seems to be up and running but when checking the logs from time to time I get a message of this type:

```bash
Error mining Block: BlockAppendError(Block is not a child of the last block,Block(4Bk5FxnuKMPqZeh4Rfyn1pE6UyNYfznQd7MtUhKtNug15WoxvhkjtCeo4AVMAW2AEXFw2DMfxd1MZ3G71SiJdnUC -> 245bVsJ..., txs=0, features=Set()))
```

When this happens, your node most likely went out-of-sync, in order to fix it you should follow the next steps:

```bash
# stop your node (if spinned up by docker compose)
$ docker-compose down

# sync your node's clock with NTP server
$ sudo service ntp stop
$ sudo ntpdate pool.ntp.org
$ sudo service ntp start

# spin up your node again
$ docker-compose up -d
```

## 2. My public node is up and running but I am not minting any blocks

Check to see if the address you configured for your node is actually your staking address:

Go to `localhost:6869/wallet/addresses` (or wherever you have your node set up). It should return your staking address. 

If this doesn't return your staking address, you must **check and fix the seedphrase in your docker-compose.yml** file, as it most likely isn't correct. Please pay attention to the following details:
* no quotation marks should be used
* each word should be seperated by a single space
* each word in lowercase
* check for mistakes in the words themselves
* no leading or trailing spaces

After checking and fixing the above, close your docker instance if you haven't done so yet, **delete any data folder you may have created already**, and restart:

```bash
# stop your node (if spinned up by docker compose)
$ docker-compose down

#remove the local volume, as it contains the seed phrase, 
#in this example it's ./data but it depends on your specific volume configuration
rm -r ./data

# spin up your node again, it will resync from genesis so it might take a while
$ docker-compose up -d
```
Now check `localhost:6869/wallet/addresses` again to see if it returns the correct address.


