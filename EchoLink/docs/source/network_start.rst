Steps to set up the EKO network and dynamically manage network authorities:
---------------------------------------------------------------------------

1.  Use the validator set contracts available at https://github.com/parity-contracts/kovan-validator-set

2.  We just need these contracts:

  + OwnedSet.sol
  + BaseOwnedSet.sol
  + ValidatorSet.sol
  + Owned.sol

3. Setup the basic chain genesis saved under genesis.json with all required fields for Authority Round consensus engine

.. sourcecode:: bash

    {
      "name": "DemoPoA",
      "engine": {
        "authorityRound": {
          "params": {
            "stepDuration": "1",
            "validators" : {
              "list": []
            }
          }
        }
      },
      "params": {
        "gasLimitBoundDivisor": "0x400",
        "maximumExtraDataSize": "0x20",
        "minGasLimit": "0x1388",
        "networkID" : "0x2323",
        "eip155Transition": 0,
        "validateChainIdTransition": 0,
        "eip140Transition": 0,
        "eip211Transition": 0,
        "eip214Transition": 0,
        "eip658Transition": 0
      },
      "genesis": {
        "seal": {
          "authorityRound": {
            "step": "0x0",
            "signature": "0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
          }
        },
        "difficulty": "0x20000",
        "gasLimit": "0x5B8D80"
      },
      "accounts": {
        "0x0000000000000000000000000000000000000001": { "balance": "1", "builtin": { "name": "ecrecover", "pricing": { "linear": { "base": 3000, "word": 0 } } } },
        "0x0000000000000000000000000000000000000002": { "balance": "1", "builtin": { "name": "sha256", "pricing": { "linear": { "base": 60, "word": 12 } } } },
        "0x0000000000000000000000000000000000000003": { "balance": "1", "builtin": { "name": "ripemd160", "pricing": { "linear": { "base": 600, "word": 120 } } } },
        "0x0000000000000000000000000000000000000004": { "balance": "1", "builtin": { "name": "identity", "pricing": { "linear": { "base": 15, "word": 3 } } } }
      }
    }


4. Setup a node by creating a node config file saved under node.toml

.. sourcecode:: bash

    [parity]
    chain = "genesis.json"
    base_path = "./eko_node_data"
    [network]
    port = 30300
    [rpc]
    cors = ["*"]
    interface ="0.0.0.0"
    port = 8545
    apis = ["web3", "eth", "net", "personal", "parity", "eko_set", "traces", "rpc", "eko_accounts"]
    [ui]
    interface = "0.0.0.0"
    port = 8181
    [websockets]
    port = 8451

5. Start the node using ``./eko --config node.toml --nat none``

6. Attach the geth to the exposed RPC port of the node using ``geth attach http://localhost:8545``.

7. Create a new account using ``personal.newAccount()`` and an account password.

8. Create a file with the node passwords saved as node.pwds and add the password ( assuming the password was "eko" ) as a list

.. sourcecode:: bash

    > eko

9. Update the node.toml as follows ( assuming the generated address address is ``0x00f3b949bb87ae90574c22f986c34207157b66b2`` )

.. sourcecode:: bash

    [parity]
    chain = "genesis.json"
    base_path = "./eko_node_data"
    [network]
    port = 30300
    [rpc]
    cors = ["*"]
    interface ="0.0.0.0"
    port = 8545
    apis = ["web3", "eth", "net", "personal", "parity", "eko_set", "traces", "rpc", "eko_accounts"]
    [ui]
    interface = "0.0.0.0"
    port = 8181
    [websockets]
    port = 8451
    [account]
    password = ["node.pwds"]
    [mining]
    engine_signer = "0x00f3b949bb87ae90574c22f986c34207157b66b2"
    reseal_on_txs = "none"

10. Restart the node using ``./eko --config node.toml --nat none``

11. Update the validator section of the genesis.json as follows

.. sourcecode:: bash

    "validators": {
       "safeContract": "0x0000000000000000000000000000000000000005"
    }

Such that the final genesis file becomes

.. sourcecode:: bash

    {
      "name": "EKOPoA",
      "engine": {
        "authorityRound": {
          "params": {
            "gasLimitBoundDivisor": "0x400",
            "stepDuration": "1",
            "validators": {
              "safeContract": "0x0000000000000000000000000000000000000005"
            }
          }
        }
      },
      "params": {
        "gasLimitBoundDivisor": "0x400",
        "maximumExtraDataSize": "0x20",
        "minGasLimit": "0x1388",
        "networkID": "0x2323",
        "eip155Transition": 0,
        "validateChainIdTransition": 0,
        "eip140Transition": 0,
        "eip211Transition": 0,
        "eip214Transition": 0,
        "eip658Transition": 0
      },
      "genesis": {
        "seal": {
          "authorityRound": {
            "step": "0x0",
            "signature": "0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
          }
        },
        "difficulty": "0x20000",
        "gasLimit": "0x56691B7"
      },
      "accounts": {
        "0x0000000000000000000000000000000000000001": { "balance": "1", "builtin": { "name": "ecrecover", "pricing": { "linear": { "base": 3000, "word": 0 } } } },
        "0x0000000000000000000000000000000000000002": { "balance": "1", "builtin": { "name": "sha256", "pricing": { "linear": { "base": 60, "word": 12 } } } },
        "0x0000000000000000000000000000000000000003": { "balance": "1", "builtin": { "name": "ripemd160", "pricing": { "linear": { "base": 600, "word": 120 } } } },
        "0x0000000000000000000000000000000000000004": { "balance": "1", "builtin": { "name": "identity", "pricing": { "linear": { "base": 15, "word": 3 } } } }
      }
    }

The address specified in the safeContract address will be the deployed address of the validator set contract.

12. Refer to the validator set contracts at https://github.com/parity-contracts/kovan-validator-set

.. sourcecode:: bash

    git clone https://github.com/parity-contracts/kovan-validator-set.git
    cd kovan-validator-set
    remixd -s contracts/ --remix-ide "https://remix.ethereum.org"

Go to http://remix.ethereum.org/

13. Open the localhost connection from the top left corner

.. figure:: images/localhost.png
   :alt: Credentials Submit

14. Select OwnedSet.sol from the list

.. figure:: images/selectOwned.png
   :alt: Credentials Submit

15. Update the OwnedSet contract constructor as follows

.. sourcecode:: bash

    constructor(address[] _initial, address _owner) BaseOwnedSet(_initial)
      public
    {
      owner = _owner;
      systemAddress = 0xffffFFFfFFffffffffffffffFfFFFfffFFFfFFfE;
    }


16. Update the Owned.sol contract owner variable declaration as follows

.. sourcecode:: bash

    address public owner;

17. Update the BaseOwnedSet.sol as follows

  - Declare ``stakeAmount`` to store the current stake amount for the authorities in the network and ``validatorStake`` to store the stakes for each authority mapped to the address as

  .. sourcecode:: bash

      uint public stakeAmount;
      mapping(address => uint) public validatorStake;

  - Define a function ``setStakeAmount`` to provide the functionality to update the stake amount

  .. sourcecode:: bash

      function setStakeAmount(uint _stakeAmount)
        external
        onlyOwner
      {
        stakeAmount = _stakeAmount;
      }

  - Declare events

  .. sourcecode:: bash

      event ValidatorAdded(address indexed validatorAddress, uint stake);
      event ValidatorRemoved(address indexed validatorAddress, uint stake);
      event CorruptValidatorRemoved(address indexed validatorAddress, uint stake);


  - Update the ``addValidator`` function as follows

  .. sourcecode:: bash

      function addValidator(address _validator)
        external
        onlyOwner
        isNotValidator(_validator)
        payable
      {
        require(msg.value == stakeAmount);

        status[_validator].isIn = true;
        status[_validator].index = pending.length;
        pending.push(_validator);
        validatorStake[_validator] = stakeAmount;

        triggerChange();

        emit ValidatorAdded(_validator, stakeAmount);
      }

  - Define a function ``removeCorruptValidator`` to add the functionality to remove a corrupt validator from the network and transfer the locked stake amount to the admin. The final function should be as follows,

  .. sourcecode:: bash

      function removeCorruptValidator(address _validator)
        external
        onlyOwner
        isValidator(_validator)
      {
        // Remove validator from pending by moving the
        // last element to its slot
        require(validatorStake[_validator] > 0);

        uint index = status[_validator].index;
        uint _stakeAmount = validatorStake[_validator];

        pending[index] = pending[pending.length - 1];
        status[pending[index]].index = index;
        delete pending[pending.length - 1];
        pending.length--;

        msg.sender.transfer(_stakeAmount);
        validatorStake[_validator] = 0;

        // Reset address status
        delete status[_validator];

        triggerChange();

        emit CorruptValidatorRemoved(_validator, _stakeAmount);
      }


  - Update the function ``removeValidator`` to add the functionality to remove a corrupt validator from the network and transfer the locked stake amount to the admin. The final function should be as follows,

  .. sourcecode:: bash

      function removeValidator(address _validator)
        external
        onlyOwner
        isValidator(_validator)
      {
        require(validatorStake[_validator] > 0);

        // Remove validator from pending by moving the
        // last element to its slot
        uint index = status[_validator].index;
        uint _stakeAmount = validatorStake[_validator];

        pending[index] = pending[pending.length - 1];
        status[pending[index]].index = index;
        delete pending[pending.length - 1];
        pending.length--;

        msg.sender.transfer(_stakeAmount);
        validatorStake[_validator] = 0;

        // Reset address status
        delete status[_validator];

        triggerChange();

        emit ValidatorRemoved(_validator, _stakeAmount);
      }

18. Select OwnedSet contract from the list of contracts to deploy.

.. figure:: images/selectOwnedFromDeploylist.png
   :alt: Credentials Submit

19. In the arguments section, ``_initial`` would contain the list of initial validators. Here you need to place the array of addresses of your validator accounts. We will use ``[“0x00f3b949bb87ae90574c22f986c34207157b66b2”]`` as we had assigned earlier to our 1st node config file. ``_owner`` should contain the address of the owner address(for this example we will be using ``0x00f3b949bb87ae90574c22f986c34207157b66b2``) to which you would like to give authority to manage authorities in the network. Also, set the stake amount for all the new authorities in the network. For now we will be setting is to be equal to 200.

.. figure:: images/deploy.png
   :alt: Credentials Submit

20. Copy the bytecode of contract along with the encoded values of input fields by clicking the briefcase button 💼.

21. Update the account section for the genesis.json as follows,

.. sourcecode:: bash

    "accounts": {
            ."0x0000000000000000000000000000000000000001": { "balance": "1", "builtin": { "name": "ecrecover", "pricing": { "linear": { "base": 3000, "word": 0 } } } },
           "0x0000000000000000000000000000000000000002": { "balance": "1", "builtin": { "name": "sha256", "pricing": { "linear": { "base": 60, "word": 12 } } } },
           "0x0000000000000000000000000000000000000003": { "balance": "1", "builtin": { "name": "ripemd160", "pricing": { "linear": { "base": 600, "word": 120 } } } },
           "0x0000000000000000000000000000000000000004": { "balance": "1", "builtin": { "name": "identity", "pricing": { "linear": { "base": 15, "word": 3 } } } },
            "<owner_address_holding_premined_ethers>": {"balance": "<provide_initial_premined_ether_balance_here>"},
            "0x0000000000000000000000000000000000000005": {"balance": "<provide_initial_balance_here>", "constructor": "<paste_byte_code_here>"}
        }

22. Restart the Eko nodes with the keys of the ``0x00f3b949bb87ae90574c22f986c34207157b66b2`` account.

23. Connect the metamask to the exposed RPC port of the nodes.

24. Import the account ``0x00f3b949bb87ae90574c22f986c34207157b66b2`` using its private key to the metamask accounts list.

25. Select Injected Web3 as the preferred environment in remix solidity browser.

.. figure:: images/selectInjectedWeb3.png
   :alt: Credentials Submit

26. Access and interact with the validator set contracts using ``At Address`` functionality of remix solidity browser, the contracts are predeployed at ``0x0000000000000000000000000000000000000005``

.. figure:: images/accessContract.png
   :alt: Credentials Submit

27. Check the current validator by calling ``getValidators`` and the owner by calling ``owner`` constant functions

.. figure:: images/getValidators.png
   :alt: Credentials Submit

28. To add a new authority in the network, Copy the genesis file and start the second node with the node config saved under node.toml (on 2nd nodes system)

.. sourcecode:: bash

    [parity]
    chain = "../genesis.json"
    base_path = "./eko_node_data1"
    [network]
    port = 30301
    [rpc]
    cors = ["*"]
    interface ="0.0.0.0"
    port = 8541
    apis = ["web3", "eth", "net", "personal", "parity", "eko_set", "traces", "rpc", "eko_accounts"]
    [ui]
    interface = "0.0.0.0"
    port = 8181
    [websockets]
    port = 8451

29. Connect the nodes, Here we will simply use curl. Obtain 1st node’s enode:

``curl --data '{"jsonrpc":"2.0","method":"parity_enode","params":[],"id":0}' -H "Content-Type: application/json" -X POST localhost:8545``

30. Add the ``result`` to node 1 (replace enode://RESULT in the command):

``curl --data '{"jsonrpc":"2.0","method":"parity_addReservedPeer","params":["enode://RESULT"],"id":0}' -H "Content-Type: application/json" -X POST localhost:8541``

Now the nodes should indicate 1/25 peers in the console, which means they are connected to each other.

31. Geth attach to the node’s RPC exposed port using ``geth attach http://localhost:8541``

32. Generate a new account using ``personal.newAccount()``. Choose a password for the account.

33. Update the node config file for the second node by assigning the generated account value ( assuming the generated account is ``0x8c7cfb7f40b7a6c4d34c7619c6075d0402112811``) to the engine_signer such that the node config file looks like,

.. sourcecode:: bash

    [parity]
    chain = "../genesis.json"
    base_path = "./eko_node_data1"
    [network]
    port = 30301
    [rpc]
    cors = ["*"]
    interface ="0.0.0.0"
    port = 8541
    apis = ["web3", "eth", "net", "personal", "parity", "eko_set", "traces", "rpc", "eko_accounts"]
    [ui]
    interface = "0.0.0.0"
    port = 8181
    [websockets]
    port = 8451
    [account]
    password = ["node.pwds"]
    [mining]
    engine_signer = "0x8c7cfb7f40b7a6c4d34c7619c6075d0402112811"
    reseal_on_txs = "none"

34. Restart the second node

35. Sign in with the owner account using the metamask and select the ``Injected Web3`` from the environment dropdown in remix solidity browser.

36. Now, access the OwnedSet.sol contract again as before using the predeployed contract address and perform the transaction ``addValidator`` with address parameter ``0x8c7cfb7f40b7a6c4d34c7619c6075d0402112811`` to add the second node’s owner address as a new authority.

.. figure:: images/addValidator.png
   :alt: Add Validator

Note: the stake amount as msg.value needs to be supplied with this transaction

37. The node logs should look like,

.. figure:: images/nodeLogOnAuthorityAddtion.png
   :alt: Node LogOn Authority Addition

38. We can check the stakes for the validator using the constant function ``validatorStake`` as,

.. figure:: images/validatorStake1.png
   :alt: Validator Stake

39. Now, if we call ``getValidator`` we should get

.. figure:: images/getValidators1.png
   :alt: Get Validators

This gives a confirmation that the new authority has been added to the network.

40. To remove an authority the owner should perform the transaction ``removeValidator`` with address parameter ``0x8c7cfb7f40b7a6c4d34c7619c6075d0402112811``. The stakes will be transferred to the authority's address from the contract and the validator will be removed from the network.

.. figure:: images/removeValidator.png
   :alt: Remove Validators

The node logs should look like,

.. figure:: images/nodeLogOnAuthorityRemoval.png
   :alt: Node LogOn Authority Removal

Now, if we call ``getValidator`` we should get

.. figure:: images/getValidators.png
   :alt: Get Validators

This gives a confirmation that the mentioned authority has been removed from the network.

When we check the stake balance now, we should get

.. figure:: images/validatorStake2.png
   :alt: Validator Stake

41. To remove a corrupt authority the owner should perform the transaction ``removeCorruptValidator`` with the address parameter ``0x8c7cfb7f40b7a6c4d34c7619c6075d0402112811``. In this case, the stake will not be transferred to the authority's address but these will be transferred to the owner's address and the validator will be removed from the network.
