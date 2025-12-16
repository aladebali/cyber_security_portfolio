#          TryHackMe__PassCode__writeup 

Room Overview

This room is classified as an easy-level challenge under the Blockchain / Web3 category.
The objective is straightforward, focusing on interacting with a smart contract to retrieve a single passcode. 
The room is short, clear, and can be completed in a minimal amount of time.

![room-image](images/title.png)


---

Execution Overview

To begin the challenge, we must first execute all the commands provided in the room exactly as given and in the correct order. These commands are required to initialize the environment, retrieve the necessary values, and interact correctly with the deployed smart contract. Executing them properly allows us to check the contract state and confirm whether the challenge has been solved.


>`RPC_URL=http://10.66.180.95:8545`

>`API_URL=http://10.66.180.95`

>`PRIVATE_KEY=$(curl -s ${API_URL}/challenge | jq -r ".player_wallet.private_key")`

>`CONTRACT_ADDRESS=$(curl -s ${API_URL}/challenge | jq -r ".contract_address")`

>`PLAYER_ADDRESS=$(curl -s ${API_URL}/challenge | jq -r ".player_wallet.address")`

>`is_solved=`cast call $CONTRACT_ADDRESS "isSolved()(bool)" --rpc-url ${RPC_URL}``

>`echo "Check if is solved: $is_solved"`

![](images/firstcommend.png)

---

⚠️ Important Note (Local Machine / VPN Users)

If you are running this challenge on your own machine (instead of the TryHackMe AttackBox) or if you are connected via VPN, please note that the cast tool is not installed by default. The cast command is part of Foundry, which must be installed manually before executing the room commands.

Foundry installation commands:

cd ~
curl -L https://foundry.paradigm.xyz | bash
source ~/.zshenv   # or open a new terminal
foundryup

After installation, verify that the tool is available:

cast --version

Once cast is successfully installed, you can proceed to run the room-provided commands normally.


---


