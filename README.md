# truffle_deploy

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
