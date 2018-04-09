# Decentralized React Chat

![](https://github.com/rodocite/decentralized-react-chat/blob/master/chatty.png)

The motivation for this project is to explore creating a decentralized app (DApp) using the Ethereum Blockchain Platform. It's my first decentralized application!

## Whisper Protocol
The project uses the [Whisper Protocol](https://github.com/ethereum/wiki/wiki/Whisper) on Ethereum. It does not require Smart Contracts. Whisper uses a blockchain along with a distributed hash table (DHT) to provide secure, non-location based routing for communications. It supports unicast, multicast, and broadcast. Whisper's focus is security and is not meant to be performant-- although it is probably sufficient in most use-cases for light communication.

## Communication Flow Summary
Two or more users would agree on a topic, a common key, and share public IDs with each other. All three are hashed together and sent into a DHT. The protocol listens to messages matching those hashes. When a match is found, the messages are routed to the parties listening.

## Project Details
The project is an unpacked `create-react-app` and uses the [Web3.js](https://github.com/ethereum/web3.js/) library to communicate with a local `geth` process. [geth](https://github.com/ethereum/go-ethereum/wiki/geth) is a golang Ethereum runtime.

#### geth
I've included the `geth` binary in the repo for convenience. A popular library [ganache](https://github.com/trufflesuite/ganache-cli) does not yet have a Whisper implementation so `geth` seemed like my only option.

The project runs `geth` in rpc development mode with websockets enabled. Websockets are required for the Whisper's subscribe functionality. Otherwise, the protocol would be limited to unicast only.

All the chat-related code is in `/src`.

#### SymLinkID, KeyPairs, Topics
The project uses a static topic `0xffaadd11` as a common topic. However, both the common hash (symLinkID) and the public IDs (KeyPairs) are generated on mount. Users will need to join each other's symLinkID -- called `Room Hash` in the application to communicate. SymLinkID, KeyPairs, Topics are required to be in hexadecimal format when being sent into the protocol.

To display identities, I've "encapsulated" each message with a `name` required from each user. The client just splits it from the converted readable string coming back from Whisper.

#### Settings - TTL, Proof of Work, etc
There are several settings for the protocol. For the most part, I kept them at default because they were ideal for a chat project. TTL, for example, is at 10 seconds. This means the message will live on the Ethereum network for that long.

I plan to start another project to go in depth with topics, hashes, pow, and ttl. Possibly a Twitter or CL style app.

#### Deploy Issues
- Since `geth` does not support SSL, you need something like nginx to be able to deploy this app over https or write your own server certificates which is beyond the scope of this project.
- There is an issue w/ Dockerizing the project due to a Web3 websocket dependency pointing to a repo without a commit hash. I'm still investigating the issue.

## Running the Project
Yarn seems to have issues with the Web3 library. Use `npm`.

```
npm install
npm start
```