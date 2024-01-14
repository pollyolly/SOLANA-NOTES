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
### Create App Folder
```
$ mkdir hello-solana
$ mkdir hello-solana/src
```
hello-solana/src/program/Cargo.toml
```
[package]
name = "program"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
solana-program = "1.9.9"

[dev-dependencies]
solana-program-test = "1.9.9"
solana-sdk = "1.9.9"

[lib]
crate-type = ["cdylib", "lib"]
```

```
$ cd hello-solana/src
$ cargo new --lib program

$ cd  hello-solana/program/
$ cargo build-bpf
```

hello-solana/src/program/src/lib.rs
```
use solana_program::{
    account_info::AccountInfo,
    entrypoint,
    entrypoint::ProgramResult,
    msg,
    pubkey::Pubkey
};
entrypoint!(process_instruction);
fn process_instruction(
        program_id: &Pubkey,
        accounts: &[AccountInfo],
        instruction_data: &[u8],
    )-> ProgramResult {
    msg!("Hello Solana! {From Rust!}");
    Ok(())
}
```
### Deploy Solana Program
```
$ cd hello-solana/src/program/
$ solana program deploy target/deploy/program.so
```
Check Configuration
```
$ solana config get
```
### Install Web3 Client
```
$ cd hello-solana
$ npm install --save @solana/web3.js
```
hello-solana/src/client/maint.ts
```vim
import {
    Keypair,
    Connection,
    PublicKey,
    LAMPORTS_PER_SOL,
    TransactionInstruction,
    Transaction,
    sendAndConfirmTransaction,
} from '@solana/web3.js';
import fs from 'mz/fs';
import path from 'path';

const PROGRAM_KEYPAIR_PATH = path.join(
    path.resolve(__dirname, '../../program'),
    'target/deploy/program-keypair.json'
);

async function main() {
    console.log("Launching cllient...");
/*
  Connect to Solana Dev Net
*/
    let connection = new Connection('https://api.devnet.solana.com', 'confirmed');
/*
  Get our program public key
*/
    const secretKeyString = await fs.readFile(PROGRAM_KEYPAIR_PATH, {encoding: 'utf8'});
    const secretKey = Uint8Array.from(JSON.parse(secretKeyString));
    const programKeypair = Keypair.fromSecretKey(secretKey);
    let programId: PublicKey = programKeypair.publicKey;

/*
    Generate account (keypair) to transact with our program
*/
    const triggerKeypair = Keypair.generate();
    const airdropRequest = await connection.requestAirdrop(
        triggerKeypair.publicKey,
        LAMPORTS_PER_SOL,
    );
    await connection.confirmTransaction(airdropRequest);

    console.log("--Pinging Program", programId.toBase58());
    const instruction = new TransactionInstruction({
        keys: [{pubkey: triggerKeypair.publicKey, isSigner: false, isWritable: true}],
        programId,
        data: Buffer.alloc(0)
    });
    await sendAndConfirmTransaction(
        connection,
        new Transaction().add(instruction),
        [triggerKeypair]
    );
        
}
main().then(
    () => process.exit(),
    err => {
        console.error(err);
        process.exit(-1);
    }
)
```
package.json
```
{
"scripts": {
  "start": "ts-node src/client/main.ts",
  "clean": "npm run clean:program",
  "build:program": "cargo build-bpf --manifest-path=./src/program/Cargo.toml --bpf-out-dir=dist/program",
  "clean:program": "cargo clean --manifest-path=./src/program/Cargo.toml && rm -rf ./dist",
  "test:program": "cargot test-bpf --manifest-path=./src/program/Cargo.toml"
},
  "dependencies": {
    	"@solana/web3.js":"^1.89.0",
    	"ts-node":"^10.9.2",
    	"mz": "^2.7.0"
  },
  "devDependencies": {
    	"@tsconfig/recommended": "^1.0.1",
    	"@types/mz": "^2.7.2",
    	"ts-node": "^10.0.0",
    	"typescript": "^4.0.5"
  }
}
```
```
$ npm install 
```
Create dist/program and Execute
```
$ npm run build:program
```
Re-deploy Solana Program 
```
$ solana program deploy ./dist/program/program.so
```
Display deployed programs
```
$ solana program show --programs
```
```
Program Id                                   | Slot      | Authority                                    | Balance
FzijVdNpxK3PDhZEi3xZHfzqTuNLc9ZgNnDUrQwgVgJo | 272245984 | 6LzTE5dYd3hNEiZ4o5X6BhUu8SNDpRC2K2taCNQnRBjc | 0.28004952 SOL
BGP3NP2bV7gjxVX8Sd5psiHocsjFpztmNd2NkGo9yZhC | 272244152 | 6LzTE5dYd3hNEiZ4o5X6BhUu8SNDpRC2K2taCNQnRBjc | 0.28004952 SOL
```
Find ProgramId in Solana Logs
```
$ solana logs | grep "FzijVdNpxK3PDhZEi3xZHfzqTuNLc9ZgNnDUrQwgVgJo invoke" - A 3 
```
Run The Program
```
$ hello-solana
$ npm run start
```
### Troubleshoot
Fix missing keypair.json (Transfer program-keypair.json and program.so*)
```
$ cd hello-solana
$ cp -r dist/program/* program/target/deploy/
```
### Reference
[Solana CLI](https://youtu.be/MiJtRAP6iOU?si=N5J9W2Sifqr-A90I)
