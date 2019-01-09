Steps to start the services
----------------------------

- Create a file ``.env`` to save the environment variables for the services
- Add and edit the following to the ``.env``

.. sourcecode:: bash

    BRIDGE_BLOCKCHAIN_HOST=
    BRIDGE_BLOCKCHAIN_PORT=
    BLOCKCHAIN_NODE_ID=
    BLOCKCHAIN_NODE_SECRET=
    MAINNET_BLOCKCHAIN_HOST=
    MAINNET_BLOCKCHAIN_PORT=
    EKO_CONTRACT_ADDRESS=

- Install ``make`` if not installed
- Run,

.. sourcecode:: bash

    make set-up


This will initialize all the services and seed the database with the admin's account.

- To start all the services, run,

.. sourcecode:: bash

    make dirty-up

You can access the services,

* `Token Bridge`_
* `Reverse Token Bridge`_
* `Eko Energy Transfer`_

.. _Token Bridge: http://fromeko.eko.computer
.. _Reverse Token Bridge: http://toeko.eko.computer
.. _Eko Energy Transfer: http://18.224.234.105:88
