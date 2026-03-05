# IoTeX Bridge Exploit PoC — Private Key Compromise

> **Educational Purpose Only** — This PoC is created for security research and education
> purposes only. It is a simplified simulation, not a fork replay against mainnet.

## Overview
- **Date:** 2026-02-21
- **Loss:** ~$4.4M + 821M CIOTX minted
- **Chain:** Ethereum / IoTeX L1
- **Category:** Access Control / Private Key Compromise

## Vulnerability
IoTeX's cross-chain bridge used a single EOA to control the TransferValidatorWithPayload contract. The attacker compromised this key, called upgrade() to deploy a malicious contract stripped of signature checks, then drained the TokenSafe ($4.4M in 9 assets) and minted 821M CIOTX. No multisig, no timelock, no circuit breaker. Funds laundered through THORChain to Bitcoin.

## Reproduction
```bash
git clone https://github.com/NomosLabs-Security/poc-iotex-2026
cd poc-iotex-2026
forge install foundry-rs/forge-std --no-git
forge test -vvvv
```

## Key Contracts
- `VulnerableTransferValidator`: Single EOA with unchecked upgrade() authority
- `TokenSafe`: Vault holding locked bridge assets (USDC, USDT, WETH, WBTC, DAI, IOTX)
- `MinterPool`: Mints wrapped CIOTX tokens
- `FixedTransferValidator`: Multisig + 48h timelock + guardian + epoch withdrawal caps

## References
- [Rekt News — IoTeX Rekt](https://rekt.news/iotex-rekt)
- [IoTeX Security Incident Update](https://iotex.io)
- [IIP-55 Governance Proposal](https://community.iotex.io)

## License

MIT — For educational use only.
