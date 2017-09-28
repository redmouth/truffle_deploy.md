# truffle_deploy

# Development
#### start TestRpc
```$ testrpc --secure -u 0 -u 1 -u 2 --gasPrice 20000```

#### How to choose an account to deploy a contract in truffle
I figured it out. in truffle.js you can specify from: field like this:

// Allows us to use ES6 in our migrations and tests.
```
require('babel-register')
module.exports = {
  networks: {
    development: {
      host: 'localhost',
      port: 8545,
      network_id: '*', // Match any network id
      from: '0xA21983B35C767CF8609D95F4886C9A18A194D8AA'
    }
  }
}
```
https://ethereum.stackexchange.com/questions/17441/how-to-choose-an-account-to-deploy-a-contract-in-truffle
<br>

#### deploy multiple dependent contracts
https://ethereum.stackexchange.com/questions/23757/deploy-multiple-contracts-with-dependencies-truffle

#### send transaction
```
var Web3 = require("../node_modules/web3/");
web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));
module.exports = function(deployer) {
  deployer.deploy(CONTRACT, "123", {from:web3.eth.accounts[0], value:1000000});
};
```
https://ethereum.stackexchange.com/questions/12778/how-to-send-some-amount-to-a-contract-in-truffle


#### deploy contract with constructor with parameters [migrations]
https://github.com/trufflesuite/truffle/issues/43
1. passing single parameter<br> ```deployer.deploy(MyContract1, 10000000000000000);```
2. passing array<br> ```deployer.deploy(MyContract2, [1506528000, 10000000000000000, 1507392000, 11000000000000000,...]);```


#### a full CrowdSale guide
https://blog.zeppelin.solutions/how-to-create-token-and-initial-coin-offering-contracts-using-truffle-openzeppelin-1b7a5dae99b6

#### MyFunction is not a function
https://ethereum.stackexchange.com/questions/12547/truffle-test-typeerror-myfunction-is-not-a-function

#### Test contract with Web3
https://citywebconsultants.co.uk/blog/blockchain/introducing-ethereum-development-part-3-testrpc-and-truffle


#### speedup: avoid compiling unchanged files when doing tests
https://github.com/trufflesuite/truffle/issues/343<br>
https://github.com/troggy/economy/commit/9cc689604e40ad317b9cae3aeaaa7b29538dbbd0#diff-04c6e90faac2675aa89e2176d2eec7d8

<br><br>
# Deploy
##### test on geth
```
$ geth account new
$ geth account list
$ geth --unlock $ADDR --password $passfilel
$ geth --rpc --rpcaddr "127.0.0.1" --rpcport "8545" --unlock addr --password


$ geth --testnet --datadir $ETHEREUM/testnet --rpc --rpcaddr 127.0.0.1 --rpcport 8545 --unlock '0, 1' --password '$ETHEREUM/files/p.txt'
```

##### set up a private Ethereum testnet blockchain using Geth
CustomGenesis.json
```
{
    "config": {
        "chainId": 15,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "difficulty": "0x1",
    "gasLimit": "0x2100000",
    "alloc": {
        "9f3bdf37f348fb82fdd4cbcffc126c9282ef319a": 
         { "balance": "0x1337000000000000000000" }     
    }
}
```

```
$ geth account new --datadir /path/to/test-net-blockchain
$ geth removedb — datadir  /path/to/test-net-blockchain
$ geth --identity "BootstrapNode" --nodiscover --rpc --rpcaddr 127.0.0.1 --rpcport "8545" --rpccorsdomain "*" --networkid 1999 --port "30303" --ipcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" --rpcapi "db,eth,net,web3" --autodag --datadir /path/to/test-net-blockchain --nat "any"  init /path/to/CustomGenesis.json console

example
$ cd ~/.ethereum/testnet
$ geth account new --datadir ~/.ethereum/testnet
$ geth account list --datadir ~/.ethereum/testnet
$ geth --identity "BootstrapNode" --nodiscover --rpc --rpcaddr 127.0.0.1 --rpcport "8545" --rpccorsdomain "*" --networkid 1999 --port "30303" --rpcapi "db,eth,net,web3" --datadir ~/.ethereum/testnet --nat "any"  init ~/.ethereum/testnet/CustomGenesis.json console

$ geth --identity "BootstrapNode" --nodiscover --rpc --rpcaddr 127.0.0.1 --rpcport "8545" --rpccorsdomain "*" --networkid 1999 --port "30303" --rpcapi "db,eth,net,web3" --datadir ~/.ethereum/testnet --nat "any" --unlock '0' --password ~/password.txt console

enable mining
$ geth --identity "BootstrapNode" --nodiscover --rpc --rpcaddr 127.0.0.1 --rpcport "8545" --rpccorsdomain "*" --networkid 1999 --port "30303" --rpcapi "db,eth,net,web3" --datadir ~/.ethereum/testnet --nat "any" --unlock '0' --password ~/password.txt --mine --minerthreads 2 console

check coinbase account balance
>web3.fromWei(eth.getBalance(eth.coinbase), "ether")
https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts

list all eth accounts
>eth.accounts
```
https://medium.com/@WWWillems/how-to-set-up-a-private-ethereum-testnet-blockchain-using-geth-and-homebrew-1106a27e8e1e

geth --identity "BootstrapNode" --genesis CustomGenesis.json --rpc --rpcaddr 127.0.0.1 --rpcport "8545" --rpccorsdomain "*" --datadir ./ --port "30303" --nodiscover --ipcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" --rpcapi "db,eth,net,web3" --autodag --networkid "*" --nat "any" console

https://souptacular.gitbooks.io/ethereum-tutorials-and-tips-by-hudson/content/private-chain.html

<br><br>
# Rinkeby testnet
```
$ wget https://www.rinkeby.io/rinkeby.json

$ geth --datadir=$HOME/.rinkeby init rinkeby.json 

$ geth account new --datadir ~/.rinkeby/

$ ln -s ~/.rinkeby/geth.ipc ~/.ethereum/

$ geth --networkid=4 --datadir=$HOME/.rinkeby --cache=512 --ethstats='yournode:Respect my authoritah!@stats.rinkeby.io'   --bootnodes=enode://a24ac7c5484ef4ed0c5eb2d36620ba4e4aa13b8c84684e1b4aab0cebea2ae45cb4d375b77eab56516d34bfbd3c1a833fc51296ff084b770b94fb9028c4d25ccf@52.169.42.101:30303 --rpc --rpcapi="personal,eth,network"

attach to the node process
$ geth attach
```
Successful Deploy
```

$ geth --rinkeby --rpc --rpcaddr "127.0.0.1" --rpcport "8545" --rpccorsdomain "*" --rpcapi "db,eth,net,web3,personal" --unlock "0,1" --password $passfile --datadir ~/.ethereum/rinkeby
```
https://gist.github.com/redmouth/17c1c551e4aa7deeaf6441faa316e8cc


<br><br>
# Deploy multiple contract
```
contract Department {

    function enroll(uint depID, address student) returns (bool ret) {
           return true;
    }
}

contract College {
    address student;
    Department dept_instance;

    function College ( address _student , address _department  ) {
        student = _student;
        dept_instance = Department(_department);
     }

    function chooseDept ( uint id ) payable returns (bool value) {
        bool ret = student.send(msg.value);
        if (!ret)
            return dept_instance.enroll(id, msg.sender);
        else
            throw;
    }
}
```
contract College depends on contract Department, so 
1. you have to deploy contract Department first, copy the address of Department
2. then when deploying contract College, paste the copied address of deployed Department to the provided CONSTRUCTOR PARAMETERS.
https://ethereum.stackexchange.com/questions/12372/how-to-deploy-multiple-contracts-which-are-dependent-in-ethereum-wallet

<br><br>
# gasPrice, gasLimit
https://ethereum.stackexchange.com/questions/25842/truffle-gas-price-and-gas-limit-on-demand-instead-of-using-fixed-amount


<br><br>
# Remove Contract
https://ethereum.stackexchange.com/questions/3794/how-to-remove-the-dao-contract-from-ethereum-wallet-mist-watch-list-as-it-is-f

<br><br>
# Watch, monitor
https://rinkeby.etherscan.io/

<br><br>
# How to participate in crowdsale
Wallets compatible with Ethereum tokens (ERC-20 standard):
```
MyEtherWallet (no download needed)
MetaMask (Firefox and Chrome browser addon)
Mist (Desktop)
Parity (Desktop)
Parity + Ledger (Hardware wallet)
imToken (iPhone)
imToken (Android)
```
https://tokenmarket.net/what-is/how-to-participate-ethereum-token-crowdsale/


<br><br>
# truffle guide, how to buy token after crowdsale is deployed in console
using truffle console to interact with node, deployed contracts...
$ truffle console
>

https://blog.zeppelin.solutions/how-to-create-token-and-initial-coin-offering-contracts-using-truffle-openzeppelin-1b7a5dae99b6
