### SOLANA-NOTES

### Installation
```
sh -c "$(curl -sSfL https://release.solana.com/v1.17.16/install)"
```
close terminal and run the path
```
export PATH="/Users/roco/.local/share/solana/install/active_release/bin:$PATH"
```
### Generate Solana Keygen
```
$solana-keygen new
```
Output:
```
===============================================================================
pubkey: HEDZ5WTLSfz5QU71dm1mgkickbZSBC21CuED8WyGwFX
===============================================================================
Save this seed phrase and your BIP39 passphrase to recover your new keypair:
second ginger priority lemon since entire snake alone brick desert tonight home
===============================================================================
```
### Solana Config Set
```
$ solana config set --keypair ~/Desktop/id.json
```
Output:
```
Config File: /Users/roco/.config/solana/cli/config.yml
RPC URL: https://api.mainnet-beta.solana.com 
WebSocket URL: wss://api.mainnet-beta.solana.com/ (computed)
Keypair Path: /Users/roco/Desktop/id.json 
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
