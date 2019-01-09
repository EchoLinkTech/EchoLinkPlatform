# Eko Energy Transfer, Token Bridge and the Reverse Token Bridge services

## Steps to start the services:

- Create a file `.env` to save the environment variables for the services
- Add and edit the following to the `.env`

```sh
BRIDGE_BLOCKCHAIN_HOST=
BRIDGE_BLOCKCHAIN_PORT=
BLOCKCHAIN_NODE_ID=
BLOCKCHAIN_NODE_SECRET=
MAINNET_BLOCKCHAIN_HOST=
MAINNET_BLOCKCHAIN_PORT=
EKO_CONTRACT_ADDRESS=
```

- Install `make` if not installed
- Run,

```sh
make set-up
```

This will initialize all the services and seed the database with the admin's account.

- To start all the services, run,

```sh
make dirty-up
```

You can access the services,

- [Token Bridge](http://fromeko.eko.computer)
- [Reverse Token Bridge](http://toeko.eko.computer)
- [Eko Energy Transfer](http://18.224.234.105:88)

## Steps to use the Eko Transfer service

- Enter the address to which the EKO Energy is to be transferred under the label `EKO To wallet address`
- Enter the amount under the label `EKO transfer amount`
- Enter your private key under the label `Your private key`
- Click on `Transfer` to perform the transfer transaction

This will transfer the provided EKO Energy to the requested address on the EKO network.

## Steps to use the Token Bridge service

- For users

  - Sign up and login to the Token Bridge service
  - Transfer the EKO tokens to the address as shown under `Send your EKO Tokens here` section
  - Get the transaction hash of the transfer transaction and submit it to the section `Give us the transaction hash`.

  The admin will now be notified with the transaction details and after verifying and processing it the user will receive the EKO Energy over the EKO network.

- For admin

  - Login to the Token Bridge service
    - Credentials:
      ```sh
      email: admin@eko.org
      password: 123456
      ```
  - Verify the transactions listed under `Submitted` section by clicking `Verify All Transaction` or verify them individually.
  - If the transaction is valid then, it will appear under the `Verified` section. Now, process the transactions by clicking `Process All Transaction` or process them individually.
  - Enter the private key to sign the transaction and perform the EKO network transaction.

  If everything is valid the transaction will go through and the user will receive the EKO Energy over the EKO network.

## Steps to use the Reverse Token Bridge service

- For users

  - Sign up and login to the Reverse Token Bridge service
  - Transfer the EKO Energy to the address as shown under `Send your EKO Energy here` section
  - Get the transaction hash of the transfer transaction and submit it to the section `Give us the transaction hash`.

  The admin will now be notified with the transaction details and after verifying and processing it the user will receive the requested EKO tokens over the main net.

- For admin

  - Login to the Reverse Token Bridge service
    - Credentials:
      ```sh
      email: admin@eko.org
      password: 123456
      ```
  - Verify the transactions listed under `Submitted` section by clicking `Verify All Transaction` or verify them individually.
  - If the transaction is valid then, it will appear under the `Verified` section. Now, process the transactions by clicking `Process All Transaction` or process them individually.
  - Enter the private key to sign the transaction and perform the main net EKO token transfer transaction.

  If everything is valid the transaction will go through and the user will receive the requested EKO token over the main net.
