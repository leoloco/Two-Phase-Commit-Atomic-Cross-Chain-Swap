# Two Phase Commit Atomic Cross Chain Swap

## Motivation
This repo contains pseudo code of a Two Phase Commit Atomic Cross Chain Swap protocol. 
This protocol is designed to swap assets accross multiple blockchains without intermediaries such as exchanges.
It tries to solve the shortcomings of HTLC based ACCS. HTLC based ACCS have two mains limitations : 
- An honest participant can end up worse of if its message gets delayed because of client crash or packet loss
- Any participant involved in a swap must be online most of the time of the swap, he has to perform multiple audit actions on several chains, and send multiple transactions on several chains.

By using Blockchain relays and Adapters, our protocol intends to solve those shortcomings without violating the atomicity rule.

## Model

**Blockchain Relays**

A blockchain relay is a smart contract hosted on a blockchain BCa that stores block headers of a blockchain BCb. This smart contract has “light client verification” capabilities of chain BCb on chain BCa.
Relayers are miners that publish block headers of chain BCb to the relay smart contract on chain BCa. Then the smart contract uses the standard verification procedure for chain BCb’s consensus algorithm to verify this block header [V. Buterin, 2016]. Once a block header has been published, verified and buried under a satisfying number of new blocks (to avoid risk of fork), it is possible to verify any transaction of chain BCb from BCa. It could be done through a high level interface presented in [Relay.pseudo](https://github.com/leoloco/Two-Phase-Commit-Atomic-Cross-Chain-Swap/blob/master/Relay.pseudo). Such implementations have already been launched in production, the main example being BTCrelay.

**Blockchain Adaptaters**

Blockchain Adapters are stand-alone, self-hosted blockchain APIs that allows you to send transactions and query different types of blockchain protocols and networks. It can be seen as an enhanced client that allows you to automate certain tasks. For example, in the case of an HTLC based ACCS, an adapter would be responsible for generating a secret, hash and store it. The adapter would also be responsible for broadcasting transactions automatically if some conditions are fulfilled. 
One flaw of the ACCS is that the blockchain client of the participants is a single point of failure. A participant, even if he tries to comply with the protocol, is subject to DDOS attacks or any type of attacks delaying the propagation of a transaction. The blockchain adapters provide a solution to this flaw because it can be decentralized and distributed over several nodes.
A template of a blockchain adapter that could be used in our protocol is presented [Adapter.pseudo](https://github.com/leoloco/Two-Phase-Commit-Atomic-Cross-Chain-Swap/blob/master/Adapter.pseudo).






