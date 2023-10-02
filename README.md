# NITKA Smart contract 

This is a simple smart contract developed using Rust with CosmWasm. The logic of this smart contract is basic: when we execute, the smart contract will increment the counter variable to one. It also has a  functionality to reset the counter in the contract to 0.

## Functionalities

### 1. Instantiate
When the smart contract is instantiated, it initializes the counter to zero and sets the contract version and name.

### 2. Execute
This contract can handle two types of messages:
- **Update:** This will increment the counter by one each time it is called.
- **Reset:** This will reset the counter to zero.

### 3. Query
The contract also has a query functionality that returns the current value of the counter.

## Tests
### 1. Instantiation Test
This test asserts that upon instantiation, the counter is set to zero.

### 2. Execute Update Test (`test_execute`)
This test performs the following:
   - It instantiates the contract.
   - Executes the `Update` message once and asserts that the counter is correctly updated to 1.
   - Executes the `Update` message again and asserts that the counter is incremented to 2.

### 3. Reset Counter Test (`test_reset_counter`)
This test performs the following:
   - It assumes that the `Update` message is called several times to increase the counter.
   - Executes the `Reset` message to reset the counter.
   - It then verifies that the reset operation has been successful by checking the counter’s value and the response attributes.
   - Finally, it loads the state and asserts that the counter is indeed reset to 0.

## Running Tests

To run these tests, navigate to the project directory and run:
```sh
cargo test
```
Results: 
```sh
~/rust/nitka-smartcontract$ cargo test
   Compiling nitka-smartcontract v0.1.0 (/home/david.casalod/rust/nitka-smartcontract)
    Finished test [unoptimized + debuginfo] target(s) in 3.05s
     Running unittests src/lib.rs (target/debug/deps/nitka_smartcontract-f097190f75288bd3)

running 4 tests
test contract::tests::test_instantiate ... ok
test execute::tests::test_reset_counter ... ok
test execute::tests::test_execute ... ok
test query::tests::test_query ... ok

test result: ok. 4 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

   Doc-tests nitka-smartcontract

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
```

## Deployment into Osmosis test-network

To evaluate the smart contract, it was deployed to the Osmosis test network. The contract was compiled, optimized using rust-optimizer, and the resultant wasm binary was stored in the Osmosis Testnet blockchain.

Here are the result of executing the contract and the interaction with the network: 

### Query to check the counter value
```sh
david.casalod@bip-dev-04:~/rust/nitka-smartcontract$  QUERY='{"counter":{}}'osmosisd query wasm contract-state smart $CONTRACT_ADDR "$QUERY" --output json

{"data":{"counter":0}}
```
### Increment contract’s count
```sh
david.casalod@bip-dev-04:~/rust/nitka-smartcontract$ TRY_UPDATE='{"update":{}}'
osmosisd tx wasm execute $CONTRACT_ADDR "$TRY_UPDATE" --from wallet --gas-prices 0.025uosmo --gas auto --gas-adjustment 1.3 -y
Enter keyring passphrase:
gas estimate: 147061
code: 0
codespace: ""
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: 6C8326335684F27C39D0E4C526BAE744909836B3C47865D671D54D693B3ECDB0
```

### Query again to check the increment in the counter value
```sh
david.casalod@bip-dev-04:~/rust/nitka-smartcontract$ osmosisd query wasm contract-state smart $CONTRACT_ADDR "$QUERY" --output json
{"data":{"counter":1}}
```

### Reset to 0
```sh
david.casalod@bip-dev-04:~/rust/nitka-smartcontract$ TRY_UPDATE='{"reset":{}}'
osmosisd tx wasm execute $CONTRACT_ADDR "$TRY_UPDATE" --from wallet --gas-prices 0.025uosmo --gas auto --gas-adjustment 1.3 -y  
Enter keyring passphrase:
gas estimate: 145631
code: 0
codespace: ""
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: 7D91492E870534956EB8B8151CC255F3861295F58CBC2D550ABD25B64B597B39
david.casalod@bip-dev-04:~/rust/nitka-smartcontract$ osmosisd query wasm contract-state smart $CONTRACT_ADDR "$QUERY" --output json
{"data":{"counter":0}}
```
Through these interactions, we observe the increment in count and its subsequent reset to 0


# Conclusion
The outlined interactions and results demonstrate the basic functionalities of the Nitka Smart Contract, showing its ability to manipulate the counter value as designed, within the Osmosis test network.
