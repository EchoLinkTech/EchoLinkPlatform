Using Docker to setup the nodes in the network
----------------------------------------------

1. Clone the EKO platform repository

2. Run

.. sourcecode:: bash

    make set-up

This will install all the dependencies and set up build the docker image for the EKO node

3. Copy the genesis configurations for the network to the ``blockchain/configuratons/genesis.json``

4. Copy the key in file format to the ``blockchain/configuratons/key.json``. If the node is set up as the miner node then this will be used.

5. Copy the password used to create the ``key.json`` to the ``blockchain/configuratons/node.pwds``.

6. Different modes:

  - Start a new network

    1. Run

    .. sourcecode:: bash

        make initialize-blockchain

    2. Type ``1`` and enter to select ``Start a new network`` from the options provided.

    3. Run

    .. sourcecode:: bash

        make dirty-up

  This will start the node with a new network configurations.

  - Join an existing network as a miner node

    1. Run

    .. sourcecode:: bash

        make initialize-blockchain

    2. Type ``2`` and enter to select ``Join an existing network as a miner node`` from the options provided.

    3. Enter the valid enode value of the node you would like to connect to from the existing network

    4. Run

    .. sourcecode:: bash

        make dirty-up

  This will start the node as a miner node and sync with the existing nodes in the network. If the validator address has been given permission to become a validator in the network the node will start mining the new blocks, else it will wait for the admin to grant permission using the Validator set contract methods.

  - Join an existing network as a viewer node

    1. Run

    .. sourcecode:: bash

        make initialize-blockchain

    2. Type ``3`` and enter to select ``Join an existing network as a viewer node`` from the options provided.

    3. Enter the valid enode value of the node you would like to connect to from the existing network

    4. Run

    .. sourcecode:: bash

        make dirty-up

  This will start the node as a viewer node and sync with the existing nodes in the network.

7. Now, follow the steps as mentioned from points ``31`` to ``41`` from the previous section.
