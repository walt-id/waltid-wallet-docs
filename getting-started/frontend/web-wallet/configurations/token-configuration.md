# Token-related configuration

Token-related configurations hold token auxiliary data mapping. Currently, there are 2 configurations
available:
- [chain explorer configuration](#chain-explorer-configuration)
- [marketplace configuration](#marketplace-configuration)

## Chain explorer configuration

Chain explorer configuration holds the mapping of chains to their respective explorer location.
It is stored in the `chainexplorer.conf` file.

e.g. chain explorer configuration
```hocon
explorers = [
    {chain = "ethereum", url = "https://etherscan.io/address/%s"},
    {chain = "polygon", url = "https://polygonscan.com/address/%s"},
    {chain = "mumbai", url = "https://mumbai.polygonscan.com/address/%s"},
    {chain = "tezos", url = "https://tzkt.io/%s/operations"},
    {chain = "ghostnet", url = "https://ghostnet.tzkt.io/%s/operations"},
]
```

## Marketplace configuration

Marketplace configuration holds the mapping of chains to their respective marketplace location.
It is stored in the `marketplace.conf` file.

e.g. marketplace configuration
```hocon
marketplaces = [
    {chain = "ethereum", name = "OpenSea", url = "https://opensea.io/assets/ethereum/%s/%s"},
    {chain = "polygon", name = "OpenSea", url = "https://opensea.io/assets/matic/%s/%s"},
    {chain = "tezos", name = "Rarible", url = "https://rarible.com/token/tezos/%s/%s"},
    {chain = "flow", name = "FlowVerse", url = "https://nft.flowverse.co/marketplace/%s/%s"},
    {chain = "unique", name = "Unique", url = "https://unqnft.io/market/%s/%s"},
]
```