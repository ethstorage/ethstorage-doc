# Configurations

If this is your first time installing an es-node, you'll need to configure it by creating a data file for each shard using the `es-node init` command. After that, you can start the es-node with various options.

During devnet tests, the `run.sh` file already includes the proper options for your convenience, but you should be prepared to specify configurations as needed.

## Configuration to create data files

With `es-node init` command, you can init your es-node by creating a data file for each shard.

You can specify `shard_len` (the number of shards) or `shard_index` (the index of specified shards, and you can specify more than one) to create shards that you would like to mine. If both appear, `shard_index` takes precedence. 

Here are the options that you can use with `init` command:

|Name|Description|Default|Required|
|---|---|---|---|
|`--datadir`|Data directory for the storage files, databases and keystore||✓|
|`--encoding_type`|Encoding type of the shards. 0: no encoding, 1: keccak256, 2: ethash, 3: blob poseidon. Default: 3|||
|`--shard_len`|Number of shards to mine. Will create one data file per shard.|||
|`--shard_index`|Indexes of shards to mine. Will create one data file per shard.|||
|`--l1.rpc`|Address of L1 User JSON-RPC endpoint to use (eth namespace required)||✓|
|`--storage.l1contract`|Storage contract address on l1||✓|
|`--storage.miner`|Miner's address to encode data and receive mining rewards||✓|

## Configuration to run es-node

The full list of options that you can use to configure an es-node is as follows:

|Name|Description|Default|Required|
|---|---|---|---|
|`--datadir`|Data directory for the storage files, databases and keystore||✓|
|`--download.dump`|Where to dump the downloaded blobs|||
|`--download.start`|Block number which the downloader download blobs from|`0`||
|`--l1.beacon`|Address of L1 beacon chain endpoint to use||✓|
|`--l1.beacon-based-slot`|A slot number in the past time specified by l1.beacon-based-time|`0`|✓|
|`--l1.beacon-based-time`|Timestamp of a slot specified by l1.beacon-based-slot|`0`|✓|
|`--l1.beacon-slot-time`|Slot time of the L1 beacon chain|`12`||
|`--l1.chain_id`|Chain id of L1 chain endpoint to use|`1`||
|`--l1.epoch-poll-interval`|Poll interval for retrieving new L1 epoch updates such as safe and finalized block changes. Disabled if 0 or negative.|`6m24s`||
|`--l1.min-duration-blobs-request`|Min duration for blobs sidecars request|`1572864`||
|`--l1.rpc`|Address of L1 User JSON-RPC endpoint to use (eth namespace required)||✓|
|`--l2.chain_id`|Chain id of L2 chain endpoint to use|`3333`||
|`--log.color`|Color the log output if in terminal mode|||
|`--log.format`|Format the log output. Supported formats: 'text', 'terminal', 'logfmt', 'json'|`text`||
|`--log.level`|The lowest log level that will be output|`info`||
|`--metrics.enable`|Enable metrics|||
|`--miner.enabled`|Storage mining enabled|||
|`--miner.gas-price`|Gas price for mining transactions|||
|`--miner.min-profit`|Minimum profit for mining transactions|`0`||
|`--miner.priority-gas-price`|Priority gas price for mining transactions|||
|`--miner.threads-per-shard`|Number of threads per shard|`runtime.NumCPU() x 2`||
|`--miner.zk-prover-mode`|ZK prover mode, 1: one proof per sample, 2: one proof for multiple samples|`2`||
|`--miner.zk-prover-impl`|ZK prover implementation, 1: snarkjs, 2: go-rapidsnark|`1`||
|`--miner.zkey`|zkey file name with path|`./build/bin/snark_lib/zkey/blob_poseidon2.zkey`||
|`--network`|Predefined L1 network selection. Available networks: devnet|||
|`--p2p.advertise.ip`|The IP address to advertise in Discv5, put into the ENR of the node. This may also be a hostname / domain name to resolve to an IP.|||
|`--p2p.advertise.tcp`|The TCP port to advertise in Discv5, put into the ENR of the node. Set to p2p.listen.tcp value if 0.|`0`||
|`--p2p.advertise.udp`|The UDP port to advertise in Discv5 as fallback if not determined by Discv5, put into the ENR of the node. Set to p2p.listen.udp value if 0.|`0`||
|`--p2p.ban.peers`|Enables peer banning. This should ONLY be enabled once certain peer scoring is working correctly.|||
|`--p2p.bootnodes`|Comma-separated base64-format ENR list. Bootnodes to start discovering other node records from.|||
|`--p2p.disable`|Completely disable the P2P stack, if P2P is disabled, mining will not start automatically.|||
|`--p2p.discovery.path`|Discovered ENRs are persisted in a database to recover from a restart without having to bootstrap the discovery process again. Set to 'memory' to never persist the peerstore.|`esnode_discovery_db`||
|`--p2p.listen.ip`|IP to bind LibP2P and Discv5 to|`0.0.0.0`||
|`--p2p.listen.tcp`|TCP port to bind LibP2P to. Any available system port if set to 0.|`9222`||
|`--p2p.listen.udp`|UDP port to bind Discv5 to. Same as TCP port if left 0.|`0`||
|`--p2p.nat`|Enable NAT traversal with PMP/UPNP devices to learn external IP.|||
|`--p2p.request.size`|p2p request size is the initial number of bytes to request from a remote peer. Its value should not be larger than 8 * 1024 * 1024. The request size will be changed according to network condition between the remote peer and the localnode.|1048576||
|`--p2p.sync.concurrency`|sync concurrency is the number of chunks to split a shard into to allow concurrent retrievals.|16||
|`--p2p.fill-empty.concurrency`|fill empty concurrency is the number of threads to concurrently fill encoded empty blobs.|NumCPU - 2||
|`--p2p.no-discovery`|Disable Discv5 (node discovery)|||
|`--p2p.peers.grace`|Grace period to keep a newly connected peer around, if it is not misbehaving.|`30s`||
|`--p2p.peers.hi`|High-tide peer count. The node starts pruning peer connections slowly after reaching this number.|`30`||
|`--p2p.peers.lo`|Low-tide peer count. The node actively searches for new peer connections if below this amount.|`20`||
|`--p2p.peerstore.path`|Peerstore database location. Persisted peerstores help recover peers after restarts. Set to 'memory' to never persist the peerstore. Peerstore records will be pruned / expire as necessary. Warning: a copy of the priv network key of the local peer will be persisted here.|`esnode_peerstore_db`||
|`--p2p.priv.path`|Read the hex-encoded 32-byte private key for the peer ID from this txt file. Created if not already exists.Important to persist to keep the same network identity after restarting, maintaining the previous advertised identity.|`esnode_p2p_priv.txt`||
|`--p2p.score.bands`|Sets the peer score bands used primarily for peer score metrics. Should be provided in following format: \<threshold>:\<label>;\<threshold>:\<label>;...For example: -40:graylist;-20:restricted;0:nopx;20:friend;|`-40:graylist;-20:restricted;0:nopx;20:friend;`||
|`--p2p.scoring.peers`|Sets the peer scoring strategy for the P2P stack. Can be one of: none or light.Custom scoring strategies can be defined in the config file.|`none`||
|`--p2p.scoring.topics`|Sets the topic scoring strategy. Can be one of: none or light.Custom scoring strategies can be defined in the config file.|`none`||
|`--p2p.sequencer.key`|Hex-encoded private key for signing off on p2p application messages as sequencer.|||
|`--p2p.static`|Comma-separated multiaddr-format peer list. Static connections to make and maintain, these peers will be regarded as trusted.|||
|`--p2p.test.simple-sync.end`|End of simple sync|`0`||
|`--p2p.test.simple-sync.start`|Start of simple sync|`0`||
|`--rollup.config`|Rollup chain parameters|||
|`--rpc.addr`|RPC listening address|`127.0.0.1`||
|`--rpc.escall-url`|RPC EsCall URL|`http://127.0.0.1:8545`||
|`--rpc.port`|RPC listening port|`9545`||
|`--signer.address`|Address the signer is signing transactions for|||
|`--signer.endpoint`|Signer endpoint the client will connect to|||
|`--signer.hdpath`|HDPath is the derivation path used to obtain the private key for mining transactions|||
|`--signer.mnemonic`|The HD seed used to derive the wallet's private keys for mining. Must be used in conjunction with HDPath.|||
|`--signer.private-key`|The private key to sign a mining transaction|||
|`--state.upload.url`|API that update es-node state to, the node will upload state to API for statistic if it has been set correctly.|||
|`--storage.files`|File paths where the data are stored||✓|
|`--storage.l1contract`|Storage contract address on l1||✓|
|`--storage.miner`|Miner's address to encode data and receive mining rewards||✓|


Check [here](/storage-provider-guide/storage-provider-faq.md#how-can-i-change-the-default-configurations) for the detailed operation to change the default configurations.
