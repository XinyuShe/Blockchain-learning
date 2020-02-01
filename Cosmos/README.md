# Cosmos

## Motivation：

(1）Storage layer: data is too large


(2) Network layer: Blockchain performance X（throughput--TPS）

(3）Consensus layer: traditional consensus algorithm is less efficient


**Cosmos pros:**

1) Make it easier to create a new chain by using modeled cosmos-SDK

2) Under the premise of retaining their own governance, Cosmos allows the blockchain to convert values between each other through IBC and Peg-Zone.

3) Enables blockchain applications to have both horizontal and vertical scalability.

4) More importantly, Cosmos is not a product. It is an ecosystem built on a modular, programmable and interchangeable tool. 

**Cosmos cons：**

1）Cosmos Hub（https://hub.cosmos.network/） is centralized，and Security requirements are greatly improved.

2）In actual operation, most of the blocks have no transactions, which wastes space.

## 1. Project Composition

Cosmos can be divided into cosmos (cosmos-SDK) and Tendermint, which are also divided into two projects on github.

(1) cosmos-SDK（https://github.com/cosmos/cosmos-sdk）
![](https://raw.githubusercontent.com/Billy1900/documentation/master/cosmos/img/cosmosSDK-inc.png)

- Baseapp: The basic ABCI application definition template so that the Cosmos-SDK application can communicate with the underlying Tendermint node.
- Client: client CLI and REST server tools for interacting with the SDK application
- Examples: Examples of how to build standalone applications.
- Server: The full node server running the SDK application on Tendermint.
- Store: SDK database - MyKLE multi-storage supports multiple types of Melkey key value storage.
- Types: Common types in SDK applications.
- x: An extension to the core where all messages and handlers are defined. A little mysterious taste. 

(2) Tendermint（https://github.com/tendermint/tendermint） 
![](https://raw.githubusercontent.com/Billy1900/documentation/master/cosmos/img/tendermint-inc.png)
- Blockchain: Rule validation and related data structures for the Tendermint chain structure.
- State: State management trace, including signature summaries and the most wanted Merkel proofs.
- Consensus: The consensus part is mainly based on the DPOS algorithm of Byzantine BFT.
- Mempool: A storage pool that verifies the completed transaction.
- Networks: local and remote network services.
- Node: block node and related data structure. Similar to Ethereum, abstraction of a layer.
- Lite: Light node, used to verify the header.
- P2p: Network layer for chain discovery and governance.
- Proxy: proxy layer, used for transaction and other proxy connection verification consensus.
- Evidence: Storage certificate related.
- Rpc: Remote communication interface.
- Types: Basic data type definitions. 
![](http://www.maiyaotop.com/wp-content/uploads/2019/05/970db67da53c63357cce07255008b618.png)

## 2. Cosmos
（https://github.com/cosmos/cosmos/blob/master/WHITEPAPER.md）

Cosmos is an independent parallel blockchain network in which each blockchain runs through a classical Byzantine fault-tolerant consensus algorithm such as Tendermint1. The first blockchain in the network will be the Cosmos hub. The Cosmos hub connects to many other blockchains (or call them partitions) through a new blockchain inter-chain communication protocol. The Cosmos hub can track the variety of tokens and record the total number of tokens in each connected partition. Tokens can be safely and quickly moved from one partition to another without exchange currency between them, as token transfers between all partitions pass through the Cosmos hub.

The certifier roles in Cosmos are similar to Bitcoin miners, but they use cryptographic signatures to vote. The certifier is a secure machine dedicated to submitting blocks. Non-verifiers can entrust an equity token (also known as an "atom" coin) to any verifier to earn a certain block fee and an atom award, but if the verifier is hacked or violates the agreement, the token will Face the risk of being punished (cut). Tendermint's provable security mechanisms for Byzantine consensus, as well as collateral guarantees for stakeholders (verifiers and principals), provide verifiable and quantifiable security for nodes and even light clients.

![](https://raw.githubusercontent.com/gnuclear/atom-whitepaper/master/images/hub_and_zones.png)

On this basis, the Cosmos Hub manages a number of independent blockchains (sometimes called "shards" called "partitions", with reference to "shards" from well-known database extension techniques). Fragments on the hub will continuously submit the latest blocks, which allows the hub to synchronize the state of each partition. Similarly, each partition will be consistent with the state of the hub (although the partitions will not be synchronized with each other unless indirectly through the hub). Prove that the message is accepted and sent by issuing a Merkel certificate to pass the message from one partition to another. This mechanism is called "inter-block communication" or simply "IBC" mechanism.

![](https://img.chainnews.com/material/images/349c109aa5f3f4a4f4a5ba20ff59e793.jpg)

- Cosmos takes advantage of the scalability of two classes:

Vertical expansion: This includes methods for extending the blockchain itself. By moving away from the PoW mechanism and optimizing other components, the Tendermint consensus engine can achieve thousands of transactions per second. As a result, applications such as EVM have a much lower transaction throughput than applications that embed transaction types and state transition functions (such as standard Cosmos-SDK applications). This is why it makes sense to customize the blockchain for the application (as explained further here).

Horizontal scaling: Even if the consensus engine and application are highly optimized, at a certain point in time, the transaction throughput of a single chain will inevitably encounter bottlenecks. This is a limitation of horizontal expansion. To go beyond it, we must introduce a multi-chain architecture. The idea is to have multiple parallel chains running the same application and operate together alongside people, making the blockchain theoretically unlimited.

## 3. Tendermint
(https://tendermint.com/docs/)
Paper (https://arxiv.org/abs/1807.04938)

Tendermint consists of two chief technical components: a blockchain consensus engine and a generic application interface. The consensus engine, called Tendermint Core. The application interface, called the Application BlockChain Interface (ABCI)

**(1) Tendermint**
Tendermint's goal is to provide a generic engine that includes a network layer and a consensus layer to easily build arbitrary applications on top of it.
Tendermint roles: (the definition maybe not right)
Validator，delegator，learner (receive blocks), sentry node (guardians of validator + access network)

![](https://tendermint.com/docs/assets/img/tm-transaction-flow.258ca020.png)

- Rotating Leader Election: (tendermint/types/validator_set.go)
Tendermint rotates through the validator set, i.e. block proposers, in a weighted round-robin fashion. The more stake, i.e. voting power, that a validator has delegated to them, the more weight that they have, and the proportionally more times they will be elected as leaders. To illustrate, if one validator has the same amount of voting power as another validator, they will both be elected by the protocol an equal amount of times.

>The simplified explanation of how the algorithm works looks like this:
1) Validator weight is established
2) Validator is elected, their turn to propose a block
3) Weight is recalculated, decreases some amount after round is complete
4) As each round progresses, weight increases incrementally in proportion to voting power
5) Validator is selected again
And here is the actual code snippet on: 
(https://github.com/tendermint/tendermint/blob/master/types/validator_set.go#L50)

Because the protocol selects block proposers deterministically, given that you know the validator set and each validator’s voting power, you could compute exactly who the next block proposers will be in rounds x, x + 1,...,x + n. Because of this, critics argue that Tendermint isn't decentralized enough. When you can know predictably who the leaders will be, an attacker could target those leaders and launch a DDoS attack against them and potentially halt the chain from progressing. We mitigate this attack vector by implementing something called Sentry Architecture in Tendermint.

Be noticeable about the Rest, rpc, P2P mode in the figure below.
![](https://cdn.ktvsky.com/8066c0eda0f1e1b57fbe2ecec45cc1d9.png)

**(2) ABCI：**(https://github.com/tendermint/abci#message-types)

![](https://tendermint.com/docs/assets/img/abci.3542de28.png)

Bridging is not based on the Tendermint mechanism chain: In the Cosmos architecture, we have shown how Tendermint-based blockchains are chained. But Cosmos is not limited to connecting Tendermint-based blockchains. In fact, any blockchain can be connected to Cosmos.

We need to distinguish the blockchain of real-time finality and probability end.
1) Real-time final blockchain

A blockchain using a real-time final consensus algorithm can be connected to Cosmos by using the IBC protocol. For example, if Ethereum switches to Casper FFG (Friendly Finality Gadget), it can use IBper to connect directly to the Cosmos ecosystem using IBC.

2) Probability final blockchain

It can be a bit tricky for blockchains that don't have real-time finality (for example, blockchains based on proof of effort). For these chains, we use a special chain of agents called Peg-Zon.

Peg-Zone is a blockchain that tracks the status of another blockchain. Peg-Zone itself has real-time finality, so it is compatible with IBC. Its role is to establish a finality for the bridged blockchain. For example, we want to bridge the EW based PoW-based Ethereum so that we can send tokens back and forth between Ethereum and Cosmos. Because PoW-based Ethereum does not have real-time finality, we need to create a Peg-Zone （https://github.com/cosmos/peggy/tree/master/spec） to bridge it.


## 4. IBC
The connection between the blockchains based on Tendermint is implemented by a protocol called IBC (Inter-Blockchain Communication Protocol). IBC takes advantage of the immediate final deterministic nature, allowing heterogeneous blockchains to exchange items (such as Tokens) with each other. Let's take a closer look at how IBC works and how to create blockchain internet: Cosmos

Cross-chain trading process:
![](https://raw.githubusercontent.com/gnuclear/atom-whitepaper/master/msc/ibc_with_ack_timeout.png)

How does IBC work?

The principle of IBC is quite simple. As an example, an account on the A chain would like to send 10 tokens (denoted as X) to the account on the B chain. These tokens are locked on chain A and then the 10 Xs are transferred from chain A to chain B. The B chain tracks the certifier of the A chain. If two-thirds of the chain A validates the signature of the person, it is valid and creates 10 Xs on the B chain.

Note that the token created on the B chain is not actually native X, since X exists only on the A chain. They actually become tokens in the B chain, and X is frozen on the A chain and cannot be used.

Use the same mechanism to unlock the token when they return to the original chain. If you want a more comprehensive understanding of the IBC agreement, you can check out this detail. It is worth mentioning that the IBC protocol we describe is only used for token transmission, that is, sending tokens from one chain to another, but may also be extended to support the transmission of logical operations.

If there is still something you don’t understand, please read in Tendermint documentation.

## 5. cosmos-sdk documentation 
（https://cosmos.network/docs/）
