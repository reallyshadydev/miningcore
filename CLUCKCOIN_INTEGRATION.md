# CluckCoin Integration with Miningcore

This document describes how to configure Miningcore to work with CluckCoin, a Scrypt-based blockchain.

## CluckCoin Specifications

- **Name**: CluckCoin
- **Symbol**: CLCK
- **Algorithm**: Scrypt (1024, 1)
- **Family**: Bitcoin
- **Default Port**: 24390
- **Block Time**: 5 minutes (300 seconds)
- **Target Timespan**: 10 minutes (600 seconds)
- **Genesis Block Hash**: `e29ab325f5a7894045dd059fbece2b24456b26e90559274c242e7a2f5b8453c9`

## Configuration

### 1. Coin Definition

The CluckCoin configuration has been added to `src/Miningcore/coins.json` with the following parameters:

```json
"cluckcoin": {
    "name": "CluckCoin",
    "canonicalName": "CluckCoin",
    "symbol": "CLCK",
    "family": "bitcoin",
    "website": "",
    "market": "",
    "twitter": "",
    "telegram": "",
    "discord": "",
    "coinbaseHasher": {
        "hash": "sha256d"
    },
    "headerHasher": {
        "hash": "scrypt",
        "args": [1024, 1]
    },
    "blockHasher": {
        "hash": "reverse",
        "args": [
            {
                "hash": "sha256d"
            }
        ]
    },
    "posBlockHasher": {
        "hash": "reverse",
        "args": [
            {
                "hash": "scrypt",
                "args": [1024, 1]
            }
        ]
    },
    "shareMultiplier": 65536,
    "explorerBlockLink": "",
    "explorerTxLink": "",
    "explorerAccountLink": ""
}
```

### 2. Pool Configuration

A sample pool configuration is available in `examples/cluckcoin_pool.json`. Key configurations:

- **Daemon Port**: 24390 (CluckCoin's default RPC port)
- **Mining Ports**: 
  - 3042: ASIC Mining (difficulty 1024)
  - 3043: GPU Mining (difficulty 256)
- **Algorithm**: Scrypt with parameters (1024, 1)

### 3. CluckCoin Daemon Configuration

Ensure your `cluckcoin.conf` file includes the following RPC settings:

```
rpcuser=rpcuser
rpcpassword=rpcpassword
rpcport=24390
rpcallowip=127.0.0.1
server=1
daemon=1
```

## Algorithm Details

CluckCoin uses the Scrypt algorithm with the following parameters:
- **N**: 1024
- **r**: 1
- **p**: 1

This is the same Scrypt configuration used by Litecoin and many other Scrypt coins.

## Mining Stratum

The pool supports the standard Stratum protocol for Scrypt mining. Miners should connect using:

```
stratum+tcp://your-pool-ip:3042  (ASIC)
stratum+tcp://your-pool-ip:3043  (GPU)
```

## Compatible Mining Software

Since CluckCoin uses standard Scrypt, it's compatible with all Scrypt mining software:

- **ASIC Miners**: Antminer L3+, Antminer L7, etc.
- **GPU Miners**: cgminer, bfgminer, sgminer
- **CPU Miners**: cpuminer-multi

## Verification

To verify the configuration is working:

1. Start your CluckCoin daemon
2. Configure Miningcore with the provided configuration
3. Start Miningcore
4. Connect a Scrypt miner to test

## Block Explorer Integration

Once block explorers are available for CluckCoin, update the `explorerBlockLink`, `explorerTxLink`, and `explorerAccountLink` fields in the coin configuration.

## Notes

- CluckCoin uses a 5-minute block time compared to Litecoin's 2.5 minutes
- The difficulty adjustment period is 10 minutes compared to Bitcoin's 2 weeks
- Minimum payment in the example configuration is set to 10 CLCK (adjustable)
- Pool fee in the example is set to 1% (adjustable)

## Support

For CluckCoin-specific questions, refer to the CluckCoin community channels.
For Miningcore issues, refer to the Miningcore documentation and community.