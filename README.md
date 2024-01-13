### SOLANA-NOTES

### Installation
```
sh -c "$(curl -sSfL https://release.solana.com/v1.17.16/install)"
```
close terminal and run the path in terminal
```
export PATH="/Users/roco/.local/share/solana/install/active_release/bin:$PATH"
```
### Generate Solana Keygen
```
$solana-keygen new 
```
Output:
```
Wrote new keypair to /Users/roco/.config/solana/id.json
=============================================================================
pubkey: 6LzTE5dYd3hNEiZ4o5X6BhUu8SNDpRC2K2taCNQnRBjc
=============================================================================
Save this seed phrase and your BIP39 passphrase to recover your new keypair:
adult knee tourist settle shock noise visa afraid love settle identify either
=============================================================================
```
### Solana Config Set
```
$ solana config set --keypair /Users/roco/.config/solana/id.json
```
Output:
```
Config File: /Users/roco/.config/solana/cli/config.yml
RPC URL: https://api.devnet.solana.com 
WebSocket URL: wss://api.devnet.solana.com/ (computed)
Keypair Path: /Users/roco/.config/solana/id.json 
Commitment: confirmed 
```
### Solana Test/Main/Dev Net
```
https://api.testnet.solana.com
https://api.mainnet-beta.solana.com
https://api.devnet.solana.com
```
```
$ solana config set --url https://api.devnet.solana.com
```
Output:
```
Config File: /Users/roco/.config/solana/cli/config.yml
RPC URL: https://api.devnet.solana.com 
WebSocket URL: wss://api.devnet.solana.com/ (computed)
Keypair Path: /Users/roco/Desktop/id.json 
Commitment: confirmed
```
### CLI Commands
```
$ solana            =check commands
```
```
$ solana logs       =check solana logs
$ solana balance    =check balance
$ solana airdrop 1  =requesting SOL 
```
