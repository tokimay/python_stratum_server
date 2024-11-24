# python stratum server
### simple implementation of stratum server

This is a ***Bitcoin stratum mining pool server*** </br>
who explain communicate between a miner and a stratum server. </br>
Server tested in ***main-net*** and ***reg-test*** using 
***[wildrig-multi](https://github.com/andru-kun/wildrig-multi)*** and ***[cpuminer-opt](https://github.com/JayDDee/cpuminer-opt)***.</br>
#### notice: block submitting method just tested in reg-test (if i could do that in main net i will leave git for ever :stuck_out_tongue_winking_eye: )

### valid method list
- **mining.subscribe:**
  - request body example:
  ````shell
  {"id": 1, "method": "mining.subscribe", "params": ["WildRig/0.40.8L"]}
    ```` 
  - response body example:
  ````shell
  {"id": 1, "result": [[["mining.set_difficulty", 32]], "0BFCC3F18A1605AE", 8], "error": null}
    ````
- **mining.authorize:**
  - request body example:
  ````shell
  {"id": 2, "method": "mining.authorize", "params": ["bc1q05205nxlhgwv33gnd5salpp449m2k9ye8dqg72", ""]}
    ```` 
  - response body example:
  ````shell
  {"id": 2, "result": true, "error": null}
    ````
- **mining.extranonce_subscribe**:
  - request body example:
  ````shell
  {"id": 3, "method": "mining.extranonce.subscribe", "params": []}
    ```` 
  - response body example:
  ````shell
  {"id": 3, "result": false, "error": null}
    ````
- **mining.set_difficulty:**
  - response body example:
  ````shell
  {"id": 4, "method": "mining.set_difficulty", "params": [16.0]}
    ````
- **mining.notify:**
  - response body example:
  ````shell
  {"id": 2, "method": "mining.notify", "params": ["6742d1ed000d4d29", "c35fe4e2df7c7866d06a8f94b7892106fd7eeb390002a37a0000000000000000", "01000000010000000000000000000000000000000000000000000000000000000000000000ffffffff2603294d0d21746f6b696d617940676d61696c2e636f6d", "ffffffff027a88d412000000001600147d14fa4cdfba1cc8c5136d21df8435a976ab14990000000000000000266a24aa21a9ed20cf115aea7d5949653d2935e80d4703829e59539a40e9ede927db49509614dc00000000", ["f640a577fb4ee6989fd7f239beb0e1ca7f21776f9c70dcce929162597f93b10b", "48d0d07032f9bd8d662874927847040d478a0054de0e6923e8a09cf131702ff1", "96ed6a2663ce74bb8a9579eb778eb39c9c2f307daf6a287cc4f0249d6f1222df", "2a90d63ae86657e5d3ee9c9392a8255770a6b2b1563ef8728077b8dc60153a3b", "7e025ab05dd225c9ff43624b3901333558168a9166947448ff35222245befed8", "047a0e4600fbe0d5316a3123fccc7670b68dfe75c7051d3ba74a0e5f87a0bf5a", "bc04c1e8c1ade0ae389cb6887674723398cf4fdde56137e2fe910ab0fcd8776d", "315465f9a46ca9b37ea19692c965fc6277659582cb024ed26710095ee8c1875e", "f7ae16529703835e9170f8d918bd730c720192863b7e37c94b3abd8ca1a1cb18", "4b41fd87dd0182284d76e9a22ba21757a98369e49ee26afc327e8cd3782ab1d6", "b62b23864e8223a37eedab981985df292489333b82675f2317b9b22b84ff8148", "f35ddcb02e7e55d8cddf4d89cf09767a6cc5062c71869ab37f47cb03b63ae715"], "20000000", "1702c070", "6742d1ed", true]}
    ````
- **mining.submit:**
  - request body example:
  ````shell
  {"method": "mining.submit", "params": ["bc1q05205nxlhgwv33gnd5salpp449m2k9ye8dqg72", "6742d2e1000d4d29", "0000000000000000", "6742d2e1", "bfdc580e"], "id":4}
    ```` 
  - response body example:
  ````shell
  {"id": 4, "result": false, "error": [23, "Low difficulty share.", null]}
    ````

### usage:
+ at the first step you should run [Bitcoin core](https://bitcoin.org/en/full-node) software in your machine.
+  edit [bitcoin.config](https://bitcoin.stackexchange.com/questions/11190/where-is-the-configuration-file-of-bitcoin-qt-kept)  and set valid username and password for RPC connections.
+  clone the source:
````shell
  git clone https://github.com/tokimay/python_stratum_server 
  ````
+ edit ***setting.ini*** file (you clone it in previous step) and set bitcoin.config section same as your bitcoin.config file
  + rpcuser=YOUR_RPC_USER
  + rpcpassword=YOUR_RPC_PASSWORD
  + host=WHERE_THAT_BITCOIN_CORE_IS_RUNNING
  + port=BITCOIN_CORE_PORT
  + start_difficulty=should be a multiple of 2 </br>
  It will tell to miner how start its job. </br>
  this completely [different from network difficulty](https://bitcointalk.org/index.php?topic=5474651.0). at a word just a way to ensure that workers are active and a method for pay reward to them. </br>
  + btc_address=BTC_WALLET_ADDRESS for receive reward
+ at the end just run: 
````shell
stratumServer.py
  ````