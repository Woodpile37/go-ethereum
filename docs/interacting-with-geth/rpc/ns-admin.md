---
title: admin Namespace
description: Documentation for the JSON-RPC API "admin" namespace
---

The `admin` API gives access to several non-standard RPC methods, which allows fine grained control over a Geth instance, including but not limited to network peer and RPC endpoint management.

## admin_addPeer {#admin-addpeer}

The `addPeer` administrative method requests adding a new remote node to the list of tracked static nodes. The node will try to maintain connectivity to these nodes at all times, reconnecting every once in a while if the remote connection goes down.

The method accepts a single argument, the [`enode`](https://ethereum.org/en/developers/docs/networking-layer/network-addresses/#enode) URL of the remote peer to start tracking and returns a `BOOL` indicating whether the peer was accepted for tracking or some error occurred.

| Client  | Method invocation                              |
| :------ | ---------------------------------------------- |
| Go      | `admin.AddPeer(url string) (bool, error)`      |
| Console | `admin.addPeer(url)`                           |
| RPC     | `{"method": "admin_addPeer", "params": [url]}` |

**Example:**

```js
> admin.addPeer("enode://a979fb575495b8d6db44f750317d0f4622bf4c2aa3365d6af7c284339968eef29b69ad0dce72a4d8db5ebb4968de0e3bec910127f134779fbcb0cb6d3331163c@52.16.188.185:30303")
true
```

## admin_addTrustedPeer {#admin-addtrustedpeer}

Adds the given node to a reserved trusted list which allows the node to always connect, even if the slots are full. It returns a `BOOL` to indicate whether the peer was successfully added to the list.

| Client  | Method invocation                                     |
| :------ | ----------------------------------------------------- |
| Console | `admin.addTrustedPeer(url)`                           |
| RPC     | `{"method": "admin_addTrustedPeer", "params": [url]}` |

## admin_datadir {#admin-datadir}

The `datadir` administrative property can be queried for the absolute path the running Geth node currently uses to store all its databases.

| Client  | Method invocation                 |
| :------ | --------------------------------- |
| Go      | `admin.Datadir() (string, error`) |
| Console | `admin.datadir`                   |
| RPC     | `{"method": "admin_datadir"}`     |

**Example:**

```js
> admin.datadir
"/home/john/.ethereum"
```

## admin_exportChain {#admin-exportchain}

Exports the current blockchain into a local file. It optionally takes a first and last block number, in which case it exports only that range of blocks. It returns a boolean indicating whether the operation succeeded.

| Client  | Method invocation                                                     |
| :------ | --------------------------------------------------------------------- |
| Console | `admin.exportChain(file, first, last)`                                |
| RPC     | `{"method": "admin_exportChain", "params": [string, uint64, uint64]}` |

## admin_importChain {#admin-importchain}

Imports an exported list of blocks from a local file. Importing involves processing the blocks and inserting them into the canonical chain. The state from the parent block of this range is required. It returns a boolean indicating whether the operation succeeded.

| Client  | Method invocation                                     |
| :------ | ----------------------------------------------------- |
| Console | `admin.importChain(file)`                             |
| RPC     | `{"method": "admin_importChain", "params": [string]}` |

## admin_nodeInfo {#admin-nodeinfo}

The `nodeInfo` administrative property can be queried for all the information known about the running Geth node at the networking granularity. These include general information about the node itself as a participant of the [ÐΞVp2p](https://github.com/ethereum/devp2p/blob/master/caps/eth.md) P2P overlay protocol, as well as specialized information added by each of the running application protocols (e.g. `eth`, `les`, `shh`, `bzz`).

| Client  | Method invocation                         |
| :------ | ----------------------------------------- |
| Go      | `admin.NodeInfo() (*p2p.NodeInfo, error`) |
| Console | `admin.nodeInfo`                          |
| RPC     | `{"method": "admin_nodeInfo"}`            |

**Example:**

```js
> admin.nodeInfo
{
  enode: "enode://44826a5d6a55f88a18298bca4773fca5749cdc3a5c9f308aa7d810e9b31123f3e7c5fba0b1d70aac5308426f47df2a128a6747040a3815cc7dd7167d03be320d@[::]:30303",
  id: "44826a5d6a55f88a18298bca4773fca5749cdc3a5c9f308aa7d810e9b31123f3e7c5fba0b1d70aac5308426f47df2a128a6747040a3815cc7dd7167d03be320d",
  ip: "::",
  listenAddr: "[::]:30303",
  name: "Geth/v1.5.0-unstable/linux/go1.6",
  ports: {
    discovery: 30303,
    listener: 30303
  },
  protocols: {
    eth: {
      difficulty: 17334254859343145000,
      genesis: "0xd4e56740f876aef8c010b86a40d5f56745a118d0906a34e69aec8c0db1cb8fa3",
      head: "0xb83f73fbe6220c111136aefd27b160bf4a34085c65ba89f24246b3162257c36a",
      network: 1
    }
  }
}
```

## admin_peerEvents {#admin-peerevents}

PeerEvents creates an [RPC subscription](/docs/interacting-with-geth/rpc/pubsub) which receives peer events from the node's p2p server. The type of events emitted by the server are as follows:

- `add`: emitted when a peer is added
- `drop`: emitted when a peer is dropped
- `msgsend`: emitted when a message is successfully sent to a peer
- `msgrecv`: emitted when a message is received from a peer

## admin_peers {#admin-peers}

The `peers` administrative property can be queried for all the information known about the connected remote nodes at the networking granularity. These include general information about the nodes themselves as participants of the [ÐΞVp2p](https://github.com/ethereum/devp2p/blob/master/caps/eth.md) P2P overlay protocol, as well as specialized information added by each of the running application protocols (e.g. `eth`, `les`, `shh`, `bzz`).

| Client  | Method invocation                        |
| :------ | ---------------------------------------- |
| Go      | `admin.Peers() ([]*p2p.PeerInfo, error`) |
| Console | `admin.peers`                            |
| RPC     | `{"method": "admin_peers"}`              |

**Example:**

```js
> admin.peers
[{
    caps: ["eth/61", "eth/62", "eth/63"],
    id: "08a6b39263470c78d3e4f58e3c997cd2e7af623afce64656cfc56480babcea7a9138f3d09d7b9879344c2d2e457679e3655d4b56eaff5fd4fd7f147bdb045124",
    name: "Geth/v1.5.0-unstable/linux/go1.5.1",
    network: {
      localAddress: "192.168.0.104:51068",
      remoteAddress: "71.62.31.72:30303"
    },
    protocols: {
      eth: {
        difficulty: 17334052235346465000,
        head: "5794b768dae6c6ee5366e6ca7662bdff2882576e09609bf778633e470e0e7852",
        version: 63
      }
    }
}, /* ... */ {
    caps: ["eth/61", "eth/62", "eth/63"],
    id: "fcad9f6d3faf89a0908a11ddae9d4be3a1039108263b06c96171eb3b0f3ba85a7095a03bb65198c35a04829032d198759edfca9b63a8b69dc47a205d94fce7cc",
    name: "Geth/v1.3.5-506c9277/linux/go1.4.2",
    network: {
      localAddress: "192.168.0.104:55968",
      remoteAddress: "121.196.232.205:30303"
    },
    protocols: {
      eth: {
        difficulty: 17335165914080772000,
        head: "5794b768dae6c6ee5366e6ca7662bdff2882576e09609bf778633e470e0e7852",
        version: 63
      }
    }
}]
```

## admin_removePeer {#admin-removepeer}

Disconnects from a remote node if the connection exists. It returns a boolean indicating validations succeeded. Note a `true` value doesn't necessarily mean that there was a connection which was disconnected.

| Client  | Method invocation                                    |
| :------ | ---------------------------------------------------- |
| Console | `admin.removePeer(url)`                              |
| RPC     | `{"method": "admin_removePeer", "params": [string]}` |

## admin_removeTrustedPeer {#admin-removetrustedpeer}

Removes a remote node from the trusted peer set, but it does not disconnect it automatically. It returns a boolean indicating validations succeeded.

| Client  | Method invocation                                           |
| :------ | ----------------------------------------------------------- |
| Console | `admin.removeTrustedPeer(url)`                              |
| RPC     | `{"method": "admin_removeTrustedPeer", "params": [string]}` |

## admin_startHTTP {#admin-starthttp}

The `startHTTP` administrative method starts an HTTP based JSON-RPC [API](/docs/interacting-with-geth/rpc) webserver to handle client requests. All the parameters are optional:

- `host`: network interface to open the listener socket on (defaults to `"localhost"`)
- `port`: network port to open the listener socket on (defaults to `8545`)
- `cors`: [cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) header to use (defaults to `""`)
- `apis`: API modules to offer over this interface (defaults to `"eth,net,web3"`)

The method returns a boolean flag specifying whether the HTTP RPC listener was opened or not. Please note, only one HTTP endpoint is allowed to be active at any time.

| Client  | Method invocation                                                                              |
| :------ | ---------------------------------------------------------------------------------------------- |
| Go      | `admin.StartHTTP(host *string, port *rpc.HexNumber, cors *string, apis *string) (bool, error)` |
| Console | `admin.startHTTP(host, port, cors, apis)`                                                      |
| RPC     | `{"method": "admin_startHTTP", "params": [host, port, cors, apis]}`                            |

**Example:**

```js
> admin.startHTTP("127.0.0.1", 8545)
true
```

## admin_startWS {#admin-startws}

The `startWS` administrative method starts an WebSocket based [JSON RPC](https://www.jsonrpc.org/specification) API webserver to handle client requests. All the parameters are optional:

- `host`: network interface to open the listener socket on (defaults to `"localhost"`)
- `port`: network port to open the listener socket on (defaults to `8546`)
- `cors`: [cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) header to use (defaults to `""`)
- `apis`: API modules to offer over this interface (defaults to `"eth,net,web3"`)

The method returns a boolean flag specifying whether the WebSocket RPC listener was opened or not. Please note, only one WebSocket endpoint is allowed to be active at any time.

| Client  | Method invocation                                                                            |
| :------ | -------------------------------------------------------------------------------------------- |
| Go      | `admin.StartWS(host *string, port *rpc.HexNumber, cors *string, apis *string) (bool, error)` |
| Console | `admin.startWS(host, port, cors, apis)`                                                      |
| RPC     | `{"method": "admin_startWS", "params": [host, port, cors, apis]}`                            |

**Example:**

```js
> admin.startWS("127.0.0.1", 8546)
true
```

## admin_stopHTTP {#admin-stophttp}

The `stopHTTP` administrative method closes the currently open HTTP RPC endpoint. As the node can only have a single HTTP endpoint running, this method takes no parameters, returning a boolean whether the endpoint was closed or not.

| Client  | Method invocation                |
| :------ | -------------------------------- |
| Go      | `admin.StopHTTP() (bool, error`) |
| Console | `admin.stopHTTP()`               |
| RPC     | `{"method": "admin_stopHTTP"`    |

**Example:**

```js
> admin.stopHTTP()
true
```

## admin_stopWS {#admin-stopws}

The `stopWS` administrative method closes the currently open WebSocket RPC endpoint. As the node can only have a single WebSocket endpoint running, this method takes no parameters, returning a boolean whether the endpoint was closed or not.

| Client  | Method invocation              |
| :------ | ------------------------------ |
| Go      | `admin.StopWS() (bool, error`) |
| Console | `admin.stopWS()`               |
| RPC     | `{"method": "admin_stopWS"`    |

**Example:**

```js
> admin.stopWS()
true
```
