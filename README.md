## Squads v4 program upgrades GitHub action

This action makes it possible to initialize a Solana program deployment via your CICD pipeline.
Optionally, it also allows you to update the IDL of your program as part of the same deployment transaction.

In order for the program data and IDL upgrades to work, you will need to set the authority of the program data buffer, and the program's IDL authority to your vault's address:

```sh
# program data buffer
solana program set-buffer-authority PROGRAM_DATA_BUFFER_ADDRESS --new-buffer-authority YOUR_VAULT_ADDRESS

# IDL buffer
anchor idl set-authority --program-id YOUR_PROGRAM_ID --new-authority YOUR_VAULT_ADDRESS
```

### Example usage:

A detailed example (with workflows) can be found in this example repository:

https://github.com/pileks/squads-idl-deploy-example

```yml
steps:
  - uses: actions/checkout@v4
  - name: Upgrade program
    uses: Squads-Protocol/squads-v4-program-upgrade@beta
    with:
      network-url: "https://api.mainnet-beta.solana.com" # "https://api.devnet.solana.com" for devnet, or your own endpoint
      multisig-pda: "YOUR_MULTISIG_PDA"
      multisig-vault-index: "0" # Usually 0, unless you are using a non-default vault
      program-id: "YOUR_PROGRAM_ID"
      buffer: "BUFFER_ADDRESS"
      spill-address: "SPILL_ADDRESS" # Which address receives rent from closed accounts
      name: "Test Upgrade"
      executable-data: "YOUR_PROGRAM_EXECUTABLE_DATA_ADDRESS"
      keypair: ${{ secrets.DEPLOYER_KEYPAIR }} # This keypair also needs to be able to propose transactions on your multisig vault
      idl-buffer: "IDL_BUFFER_ADDRESS"
```

The format of the Keypair needs to be a Uint8Array or a Private key.
