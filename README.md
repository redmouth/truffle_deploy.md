# truffle_deploy

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


#### speedup Avoid compilation when doing tests
https://github.com/trufflesuite/truffle/issues/343<br>
https://github.com/troggy/economy/commit/9cc689604e40ad317b9cae3aeaaa7b29538dbbd0#diff-04c6e90faac2675aa89e2176d2eec7d8
