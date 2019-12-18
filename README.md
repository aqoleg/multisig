# multisig

Ton multisig wallet.

## usage

### create private keys, collect public keys

Use /key.fif. Open termial, run command.
```
<build>/crypto/fift -I <src>/crypto/fift/lib -s key.fif <filename-base>
```
Where < filename-base > is the name of the existing .pk file without extension. If this file does not exists, it will be created. For example.
```
../build/crypto/fift -I ../src/crypto/fift/lib -s key.fif tests/wallets/0
```
Repeat this step for each key, and save all of the public keys.

### create the wallet

Use /create.fif. Run command.
```
<build>/crypto/fift -I <src>/crypto/fift/lib -s 
create.fif <workchain-id> <k> <public-key-0> [...<public-key-n>]
```
where < public-key-0 > ... < public-key-n > is the public keys from the previous step. For example.
```
../build/crypto/fift -I ../src/crypto/fift/lib -s create.fif 0 2 
71321937339207315216104512986610159878091710505727212000647474584844399853668 
71321937339207315216104512986610159878091710505727212000647474584844399853667 
71321937339207315216104512986610159878091710505727212000647474584844399853666
```
This will create 2-of-3 multisig wallet file /wallet.boc and addres file /wallet.addr. Wallet address can be accessed later with this command.
```
<build>/crypto/fift -I <src>/crypto/fift/lib -s <src>/crypto/smartcont/show-addr.fif <filename-base>
```
For example.
```
../build/crypto/fift -I ../src/crypto/fift/lib -s ../src/crypto/smartcont/show-addr.fif wallet
```
Get some grams on the non-bouncable address. Run the client and send file /wallet.boc
```
sendfile wallet.boc
```

### create transaction

Use /tx.fif. Run command.
```
<build>/crypto/fift -I <src>/crypto/fift/lib -s 
tx.fif <filename-base> <multisig-addr> <dest-addr> <seqno> <amount> <exp_time>
```
where < filename-base > is the name of the existing .pk file without extension, < amount > is the value in gram. For example.
```
../build/crypto/fift -I ../src/crypto/fift/lib -s tx.fif tests/wallets/0 
0:52a3775855ce747e626ef82b9a3ec05895132c483d1aa166bcb500d1cfb658b9 
kQBSo3dYVc50fmJu-CuaPsBYlRMsSD0aoWa8tQDRz7ZYubMD 0 0.14 2000
```
This will create file /tx.boc which can be use immediatly or after addition some extra signatures.

### add extra signatures

Transfer /tx.boc file to the owner of the different private key. Use /view.fif for get more details. Run command.
```
<build>/crypto/fift -I <src>/crypto/fift/lib -s view.fif
```
If the file /tx.boc exists it will print all details of this order. Use /add.fif for add one more signature. Run command.
```
<build>/crypto/fift -I <src>/crypto/fift/lib -s add.fif <filename-base>
```
where < filename-base > is the name of the existing .pk file without extension. If the file /tx.boc exists it will be extended with the new signature. For example.
```
../build/crypto/fift -I ../src/crypto/fift/lib -s add.fif tests/wallets/1
```
Repeat this step for each signature. Run the client and send file /tx.boc.
```
sendfile tx.boc
```

### view details in the blockchain

Run the client. View seqno.
```
runmethod <address> seqno
```
View k.
```
runmethod <address> getk
```
View n.
```
runmethod <address> getn
```
View public key.
```
runmethod <address> getkey <key_n>
```
View number of the orders.
```
runmethod <address> getordersn
```
View expiration time of the order.
```
runmethod <address> getexptime <order_n>
```
View signatures of the order.
```
runmethod <address> getsignatures <order_n>
```
View hash of the order.
```
runmethod <address> gethash <order_n>
```

## func source code

File /contract.fc contains funC source code.
File /contract.fif contains fift tvm assembler code generated from /contract.fc with this command.
```
<build>/crypto/func -o contract.fif <source>/crypto/smartcont/stdlib.fc contract.fc
```
