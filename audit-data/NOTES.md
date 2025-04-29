 Checked | Code | Files
    +    | 8    | [](./src/L1Token.sol)
    +    | 12   | [](./src/L1Vault.sol)
    +    | 17   | [](./src/TokenFactory.sol)
    -    | 64   | [](./src/L1BossBridge.sol)

----------------------------------------------------------------------------------------------

Dev notes:

- The owner of the bridge can pause operations in emergency situations.
- Because deposits are permissionless, there's an strict limit of tokens that can be deposited.
- Withdrawals must be approved by a bridge operator

## Actors/Roles

- Bridge Owner: A centralized bridge owner who can:
  - pause/unpause the bridge in the event of an emergency
  - set `Signers` (see below)
- Signer: Users who can "send" a token from L2 -> L1. 
- Vault: The contract owned by the bridge that holds the tokens. 
- Users: Users mainly only call `depositTokensToL2`, when they want to send tokens from L1 -> L2. 

## Known Issues

- We are aware the bridge is centralized and owned by a single user, aka it is centralized. 
- We are missing some zero address checks/input validation intentionally to save gas. 
- We have magic numbers defined as literals that should be constants. 
- Assume the `deployToken` will always correctly have an L1Token.sol copy, and not some [weird erc20](https://github.com/d-xo/weird-erc20)

Invariants:

- The bridge owner can pause/unpause the bridge anytime
- The users are not allow to deposit far the deposit limit
- Only the Signers can send tokens from L1 to L2
- Nobody can withdraw without the bridge owner approval.





Factory owner -(deploy)-> TokenFactory.sol
        Factory owner -(create tokens)-> TokenFactory.sol -(deploy)-> {... L1Token.sol}

Bridge Owner -(deploy)-> L1BossBridge.sol -(deploy)-> L1Vault.sol
        Bridge Owner -(pause) -> L1BossBridge.sol
        Bridge Owner -(unpause) -> L1BossBridge.sol
        Bridge Owner -(set signer) -> L1BossBridge.sol -> {... signer}

                User -(depositTokensToL2)-> L1BossBridge.sol