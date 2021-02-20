# Download Docker Image

https://drive.google.com/drive/folders/1CtuRb16zX36ZafaAZt_CpQIuW8wKsbFG?usp=sharing

# Load the docker image
```
docker load < kylin-node.tar.gz
```
# Download the docker-compose configuration file
https://github.com/Kylin-Network/kylin-node/blob/main/scripts/docker-compose.yml

By wget
`wget https://raw.githubusercontent.com/Kylin-Network/kylin-node/main/scripts/docker-compose.yml`

# Prepare the kylin-data-proxy.env file
It is in **the same directory as docker-compose.yml**.

```
KYLIN_API_KEY=xxxxxxxxxx
KYLIN_API_SECRET=xxxxxxxxxxxxx
```
# Start
```
docker-compose up -d
```
*insert-ocw* will first sleep for 30s and wait for the kylin node to start, then insert the ocw submitters. After 30s, please check the full log again.

# View logs

```
docker-compose logs
```
# View containers
```
docker-compose ps
```
# Web UI
http://localhost:3001/#/explorer

# Switch to Local Node
<img src="graphics/kylin-market-frontend.png" style="zoom:50%;" />
<img src="graphics/kylin-market-frontend-switch-local-1.png" style="zoom:50%;" />
<img src="graphics/kylin-market-frontend-switch-local-2.png" style="zoom:50%;" />

# Configuration 

```
{
  "Address": "AccountId",
  "LookupSource": "AccountId",
  "DataInfo": {
    "url": "Text",
    "data": "Text"
  }
}
```
<img src="graphics/kylin-market-frontend-developer-config-1.png" style="zoom:50%;" />

# Add a test account
If you don't have a test account TEST, use the following mnemonic seed.

```
clip organ olive upper oak void inject side suit toilet stick narrow
```
The test account is

```
5FfBQ3kwXrbdyoqLPvcXRp7ikWydXawpNs2Ceu3WwFdhZ8W4
```
<img src="graphics/kylin-market-frontend-add-test-account-1.png" style="zoom:50%;" />

<img src="graphics/kylin-market-frontend-add-test-account-2.png" style="zoom:50%;" />


# Transferring funds to a test account
<img src="graphics/kylin-market-frontend-transfer-to-test-account.png" style="zoom:50%;" />



## Insert ocw submitters

OCW requires a valid account to store the fetched data to the chain. In dev mode, you need to call `author_insertKey` to insert a valid account. If this account is not added, the following error message will be displayed at the terminal running kylin-node.

```
ERROR No local accounts available. Consider adding one via `author_insertKey` RPC.
```

Insert OCW submitters can be done on the web UI or using curl.

### **curl**

Assuming that the kylin-node service is running on port 9933 on the local machine

```
# insert aura key
curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8" -X POST  --data '{
  "jsonrpc":"2.0",
  "id":1,
  "method":"author_insertKey",
  "params": [
    "aura",
    "clip organ olive upper oak void inject side suit toilet stick narrow",
    "0x9effc1668ca381c242885516ec9fa2b19c67b6684c02a8a3237b6862e5c8cd7e"
  ]
}'

# insert gran key
curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8" -X POST  --data '{
  "jsonrpc":"2.0",
  "id":1,
  "method":"author_insertKey",
  "params": [
    "gran",
    "clip organ olive upper oak void inject side suit toilet stick narrow",
    "0xb48004c6e1625282313b07d1c9950935e86894a2e4f21fb1ffee9854d180c781"
  ]
}'

# insert ocw price_fetch key
curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8" -X POST  --data '{
  "jsonrpc":"2.0",
  "id":1,
  "method":"author_insertKey",
  "params": [
    "ocpf",
    "clip organ olive upper oak void inject side suit toilet stick narrow",
    "0x9effc1668ca381c242885516ec9fa2b19c67b6684c02a8a3237b6862e5c8cd7e"
  ]
}'

# insert ocw data_fetch key
curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8" -X POST  --data '{
  "jsonrpc":"2.0",
  "id":1,
  "method":"author_insertKey",
  "params": [
    "dftc",
    "clip organ olive upper oak void inject side suit toilet stick narrow",
    "0x9effc1668ca381c242885516ec9fa2b19c67b6684c02a8a3237b6862e5c8cd7e"
  ]
}'
```

The parameters that need to be adjusted for the call are in the param.

* KeyType: the KeyTypeId of the OCW module, since there are two different OCWs in this demo, we need to add an account for each OCW.
* Suri: the helper of the account.
* PublicKey: the public key of the account, a string starting with 0x.

After inserting the account, you also need to **fill the account with a certain balance to pay for the data uploading cost**.

### *Web UI*

*Web UI* and *curl* use the same parameters, the specific location to call is in *Developer* -> *RPC calls*, select *author* on the left, select *insertKey* in the RPC selection on the right, and then fill in the data in the corresponding dialog box on the page and then sign and submit.

# Add fetch external data source
Kylin can upload the data source to the chain through an external call (Extrinsics).

1. *Developer->Extrinsics*, 
2. the service provided by *kylinOracleModule*
3. the url of the data source needs to be converted to hex format.

```
URL: https://min-api.cryptocompare.com/data/price?fsym=BTC&tsyms=USD
Hex:0x68747470733a2f2f6d696e2d6170692e63727970746f636f6d706172652e636f6d2f646174612f70726963653f6673796d3d425443267473796d733d555344
```
<img src="graphics/kylin-market-frontend-add-external-data-sources.png" style="zoom:50%;" />

# Querying external data source results on the chain

Kylin provides a friendly GUI to query the chain data.
1. Network->Oracle data sources
2. Fill in dataId(10000000) to query the chain results of external data source.

<img src="graphics/kylin-market-frontend-query-chain-result.png" style="zoom:50%;" />

# Deploy marketplace service contract

Upload the previously compiled contract file wasm to the chain and perform the initialization, which can be done via WebUI.

For contract development and compilation, please refer to the **Compling Contracts** section of **Kylin Network Demo Tuorial** doc. 

1. *Developer->Contracts->Upload WASM*
2. upload <a href="metadata.json" target="_blank">metadata.json</a> file by `wget https://raw.githubusercontent.com/Kylin-Network/documents/main/metadata.json`  
3. upload <a href="oracle_market.wasm" target="_blank">oracle_market.wasm</a> file  `wget https://raw.githubusercontent.com/Kylin-Network/documents/main/oracle_market.wasm` 

<img src="graphics/kylin-market-frontend-deploy-market-service-coontract-1.png" style="zoom:50%;" />

<img src="graphics/kylin-market-frontend-deploy-market-service-coontract-2.png" style="zoom:50%;" />

After deploying the contract, set the contract address to the **OracleMarketAddress** field in *Settings->Developer*.

```
{
  "Address": "AccountId",
  "LookupSource": "AccountId",
  "DataInfo": {
    "url": "Text",
    "data": "Text"
  },
  "OracleMarketAddress": "xxxxxxxxxxxxxxxxxxxxxx"
}
```

# Adding Marketplace Services
Continue to call the market contract on WebUI to add new market services. Open the path as **Developer->Contracts->METADATA->addService**.

```
dataID: 10000000
name: BTC-Price --> 0x4254432d5072696365
desc: fetch BTC price from cryptocompare-->0x6665746368204254432070726963652066726f6d2063727970746f636f6d70617265
thumb:
http://6.eewimg.cn/mp/uploads/2018/08/23/25d989a0-a6b9-11e8-9783-001e676a89bd.jpg -> 0x687474703a2f2f362e656577696d672e636e2f6d702f75706c6f6164732f323031382f30382f32332f32356439383961302d613662392d313165382d393738332d3030316536373661383962642e6a7067
```

<img src="graphics/kylin-market-frontend-add-markket-service.png" style="zoom:50%;" />

# Querying Marketplace Services

*Network->Oracle marketplace*

<img src="graphics/kylin-market-frontend-query-market-service-only-btc.png" style="zoom:50%;" />
