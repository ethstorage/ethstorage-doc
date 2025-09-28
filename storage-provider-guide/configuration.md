# EthStorage Node Command Reference


The `init*.sh` and `run*.sh` helper scripts handle the common setups, but you can override any setting with the options listed below. For how to adjust default configurations, see the guide [here](/storage-provider-guide/storage-provider-faq.md#how-can-i-change-the-default-configurations).

Flags and defaults may change between releases. Always re-check with `es-node --help` and `es-node init --help` for the latest values.

## Options to init es-node

| Option | Description | Default | Environment Variable |
|--------|-------------|---------|---------------------|
| `--shard_len <value>` | Number of shards to mine. Will create one data file per shard. | 0 | N/A |
| `--encoding_type <value>` | Encoding type of the shards. 0: no encoding, 1: keccak256, 2: ethash, 3: blob poseidon. | 3 | N/A |
| `--shard_index <value>` | Indexes of shards to mine. Will create one data file per shard. | N/A | N/A |
| `--datadir <value>` | (Required) Data directory for the storage files, databases and keystore | N/A | `ES_NODE_DATADIR` |
| `--l1.rpc <value>` | (Required) Address of L1 User JSON-RPC endpoint to use (eth namespace required) | N/A | `ES_NODE_L1_ETH_RPC` |
| `--storage.l1contract <value>` | (Required) Storage contract address on l1 | N/A | `ES_NODE_STORAGE_L1CONTRACT` |
| `--storage.miner <value>` | Miner's address to encode data and receive mining rewards | N/A | `ES_NODE_STORAGE_MINER` |

## Options to run es-node

| Option | Description | Default | Environment Variable |
|--------|-------------|---------|---------------------|
| `--datadir <value>` | (Required) Data directory for the storage files, databases and keystore | N/A | `ES_NODE_DATADIR` |
| `--storage.files <value>` | (Required) File paths where the data are stored | N/A | `ES_NODE_STORAGE_FILES` |
| `--l1.rpc <value>` | (Required) Address of L1 User JSON-RPC endpoint to use (eth namespace required) | N/A | `ES_NODE_L1_ETH_RPC` |
| `--storage.l1contract <value>` | (Required) Storage contract address on l1 | N/A | `ES_NODE_STORAGE_L1CONTRACT` |
| `--storage.miner <value>` | Miner's address to encode data and receive mining rewards | N/A | `ES_NODE_STORAGE_MINER` |
| `--l1.beacon-slot-time <value>` | Slot time of the L1 beacon chain | 12 | `ES_NODE_L1_BEACON_SLOT_TIME` |
| `--l1.beacon <value>` | (Required) Address of L1 beacon chain endpoint to use | N/A | `ES_NODE_L1_BEACON_URL` |
| `--da.url <value>` | URL of the custom data availability service | N/A | `ES_NODE_DA_URL` |
| `--randao.url <value>` | URL of JSON-RPC endpoint to query randao | N/A | `ES_NODE_RANDAO_URL` |
| `--l1.min-duration-blobs-request <value>` | Min duration for blobs sidecars request | 1572864 | `ES_NODE_L1_BEACON_MIN_DURATION_BLOBS_REQUEST` |
| `--metrics.enabled` | Enable the metrics server | false | `ES_NODE_METRICS_ENABLED` |
| `--metrics.addr <value>` | Metrics listening address | "0.0.0.0" | `ES_NODE_METRICS_ADDR` |
| `--metrics.port <value>` | Metrics listening port | 7300 | `ES_NODE_METRICS_PORT` |
| `--pprof.enabled` | Enable the pprof server | false | `ES_NODE_PPROF_ENABLED` |
| `--pprof.addr <value>` | pprof listening address | "0.0.0.0" | `ES_NODE_PPROF_ADDR` |
| `--pprof.port <value>` | pprof listening port | 6060 | `ES_NODE_PPROF_PORT` |
| `--download.start <value>` | Block number which the downloader download blobs from | 0 | `ES_NODE_DOWNLOAD_START` |
| `--download.thread <value>` | Threads number that will be used to download the blobs | 1 | `ES_NODE_DOWNLOAD_THREAD` |
| `--download.dump <value>` | Where to dump the downloaded blobs | N/A | `ES_NODE_DOWNLOAD_DUMP` |
| `--l1.epoch-poll-interval <value>` | Poll interval for retrieving new L1 epoch updates such as safe and finalized block changes. Disabled if 0 or negative. | 6m24s | `ES_NODE_L1_EPOCH_POLL_INTERVAL` |
| `--rpc.addr <value>` | RPC listening address | "127.0.0.1" | `ES_NODE_RPC_ADDR` |
| `--rpc.port <value>` | RPC listening port | 9545 | `ES_NODE_RPC_PORT` |
| `--rpc.escall-url <value>` | RPC EsCall URL | "http://127.0.0.1:8545" | `ES_NODE_RPC_ESCALL_URL` |
| `--state.upload.url <value>` | API that update es-node state to, the node will upload state to API for statistic if it has been set correctly. | N/A | `ES_NODE_STATE_UPLOAD_URL` |
| `--p2p.disable` | Completely disable the P2P stack, if P2P is disabled, mining will not start automatically. | N/A | `ES_NODE_P2P_DISABLE` |
| `--p2p.no-discovery` | Disable Discv5 (node discovery) | N/A | `ES_NODE_P2P_NO_DISCOVERY` |
| `--p2p.priv.path <value>` | Read the hex-encoded 32-byte private key for the peer ID from this txt file. Created if not already exists.Important to persist to keep the same network identity after restarting, maintaining the previous advertised identity. | "esnode_p2p_priv.txt" | `ES_NODE_P2P_PRIV_PATH` |
| `--p2p.scoring.peers <value>` | Sets the peer scoring strategy for the P2P stack. Can be one of: none or light.Custom scoring strategies can be defined in the config file. | "none" | `ES_NODE_P2P_PEER_SCORING` |
| `--p2p.score.bands <value>` | Sets the peer score bands used primarily for peer score metrics. Should be provided in following format: <threshold>:<label>;<threshold>:<label>;...For example: -40:graylist;-20:restricted;0:nopx;20:friend; | "-40:graylist;-20:restricted;0:nopx;20:friend;" | `ES_NODE_P2P_SCORE_BANDS` |
| `--p2p.ban.peers` | Enables peer banning. This should ONLY be enabled once certain peer scoring is working correctly. | N/A | `ES_NODE_P2P_PEER_BANNING` |
| `--p2p.scoring.topics <value>` | Sets the topic scoring strategy. Can be one of: none or light.Custom scoring strategies can be defined in the config file. | "none" | `ES_NODE_P2P_TOPIC_SCORING` |
| `--p2p.listen.ip <value>` | IP to bind LibP2P and Discv5 to | "0.0.0.0" | `ES_NODE_P2P_LISTEN_IP` |
| `--p2p.listen.tcp <value>` | TCP port to bind LibP2P to. Any available system port if set to 0. | 9222 | `ES_NODE_P2P_LISTEN_TCP_PORT` |
| `--p2p.listen.udp <value>` | UDP port to bind Discv5 to. Same as TCP port if left 0. | 0 | `ES_NODE_P2P_LISTEN_UDP_PORT` |
| `--p2p.advertise.ip <value>` | The IP address to advertise in Discv5, put into the ENR of the node. This may also be a hostname / domain name to resolve to an IP. | N/A | `ES_NODE_P2P_ADVERTISE_IP` |
| `--p2p.advertise.tcp <value>` | The TCP port to advertise in Discv5, put into the ENR of the node. Set to p2p.listen.tcp value if 0. | 0 | `ES_NODE_P2P_ADVERTISE_TCP` |
| `--p2p.advertise.udp <value>` | The UDP port to advertise in Discv5 as fallback if not determined by Discv5, put into the ENR of the node. Set to p2p.listen.udp value if 0. | 0 | `ES_NODE_P2P_ADVERTISE_UDP` |
| `--p2p.bootnodes <value>` | Comma-separated base64-format ENR list. Bootnodes to start discovering other node records from. | N/A | `ES_NODE_P2P_BOOTNODES` |
| `--p2p.static <value>` | Comma-separated multiaddr-format peer list. Static connections to make and maintain, these peers will be regarded as trusted. | N/A | `ES_NODE_P2P_STATIC` |
| `--p2p.request.size <value>` | p2p request size is the initial number of bytes to request from a remote peer. The default value is 1 * 1024 * 1024. It is value should not larger than 8 * 1024 * 1024. The request size will be changed according to network condition between the remote peer and the local node. | 1048576 | `ES_NODE_P2P_MAX_REQUEST_SIZE` |
| `--p2p.sync.concurrency <value>` | sync concurrency is the number of chunks to split a shard into to allow concurrent retrievals. The default value is 16, the min value is 1. | 16 | `ES_NODE_P2P_MAX_CONCURRENCY` |
| `--p2p.meta.download.batch <value>` | Batch size for requesting the blob metadatas stored in the storage contract in one RPC call. | 8000 | `ES_NODE_P2P_META_BATCH_SIZE` |
| `--p2p.peers.lo <value>` | Low-tide peer count. The node actively searches for new peer connections if below this amount. | 60 | `ES_NODE_P2P_PEERS_LO` |
| `--p2p.peers.hi <value>` | High-tide peer count. The node starts pruning peer connections slowly after reaching this number. | 70 | `ES_NODE_P2P_PEERS_HI` |
| `--p2p.peers.grace <value>` | Grace period to keep a newly connected peer around, if it is not misbehaving. | 30s | `ES_NODE_P2P_PEERS_GRACE` |
| `--p2p.nat` | Enable NAT traversal with PMP/UPNP devices to learn external IP. | N/A | `ES_NODE_P2P_NAT` |
| `--p2p.peerstore.path <value>` | Peerstore database location. Persisted peerstores help recover peers after restarts. Set to 'memory' to never persist the peerstore. Peerstore records will be pruned / expire as necessary. Warning: a copy of the priv network key of the local peer will be persisted here. | "esnode_peerstore_db" | `ES_NODE_P2P_PEERSTORE_PATH` |
| `--p2p.discovery.path <value>` | Discovered ENRs are persisted in a database to recover from a restart without having to bootstrap the discovery process again. Set to 'memory' to never persist the peerstore. | "esnode_discovery_db" | `ES_NODE_P2P_DISCOVERY_PATH` |
| `--p2p.sequencer.key <value>` | Hex-encoded private key for signing off on p2p application messages as sequencer. | N/A | `ES_NODE_P2P_SEQUENCER_KEY` |
| `--p2p.test.simple-sync.start <value>` | Start of simple sync | 0 | `ES_NODE_P2P_TEST_SIMPLE_SYNC_START` |
| `--p2p.test.simple-sync.end <value>` | End of simple sync | 0 | `ES_NODE_P2P_TEST_SIMPLE_SYNC_END` |
| `--log.level <value>` | The lowest log level that will be output | "info" | `ES_NODE_LOG_LEVEL` |
| `--log.format <value>` | Format the log output. Supported formats: 'text', 'terminal', 'logfmt', 'json', 'json-pretty', | "terminal" | `ES_NODE_LOG_FORMAT` |
| `--log.color` | Color the log output if in terminal mode (defaults to true if terminal is detected) | N/A | `ES_NODE_LOG_COLOR` |
| `--signer.endpoint <value>` | Signer endpoint the client will connect to | N/A | `ES_NODE_SIGNER_ENDPOINT` |
| `--signer.address <value>` | Address the signer is signing transactions for | N/A | `ES_NODE_SIGNER_ADDRESS` |
| `--signer.mnemonic <value>` | The HD seed used to derive the wallet private keys for mining. Must be used in conjunction with HDPath. | N/A | `ES_NODE_SIGNER_MNEMONIC` |
| `--signer.hdpath <value>` | HDPath is the derivation path used to obtain the private key for mining transactions | N/A | `ES_NODE_SIGNER_ADDRESS` |
| `--signer.private-key <value>` | The private key to sign a mining transaction | N/A | `ES_NODE_SIGNER_PRIVATE_KEY` |
| `--miner.enabled` | Storage mining enabled | false | `ES_NODE_MINER_ENABLED` |
| `--miner.gas-price <value>` | Gas price for mining transactions | 0 | `ES_NODE_MINER_GAS_PRICE` |
| `--miner.priority-gas-price <value>` | Priority gas price for mining transactions | 0 | `ES_NODE_MINER_PRIORITY_GAS_PRICE` |
| `--miner.min-profit <value>` | Minimum profit for mining transactions in wei | 0 | `ES_NODE_MINER_MIN_PROFIT` |
| `--miner.zkey <value>` | zkey file name with path | "build/bin/snark_lib/zkey/blob_poseidon2.zkey" | `ES_NODE_MINER_ZKEY_FILE` |
| `--miner.zk-prover-mode <value>` | ZK prover mode, 1: one proof per sample, 2: one proof for multiple samples | 2 | `ES_NODE_MINER_ZK_PROVER_MODE` |
| `--miner.zk-prover-impl <value>` | ZK prover implementation, 1: snarkjs, 2: go-rapidsnark | 1 | `ES_NODE_MINER_ZK_PROVER_IMPL` |
| `--miner.threads-per-shard <value>` | Number of threads per shard | 24 | `ES_NODE_MINER_THREADS_PER_SHARD` |
| `--miner.email-enabled` | Enable proof submission notifications via email | false | `ES_NODE_MINER_EMAIL_ENABLED` |
| `--miner.max-gas-price <value>` | Maximum gas price for mining transactions, set to 0 to disable the check | 0 | `ES_NODE_MINER_MAX_GAS_PRICE` |
| `--archiver.enabled` | Blob archiver enabled | false | `ES_NODE_ARCHIVER_ENABLED` |
| `--archiver.addr <value>` | Blob archiver listening address | "0.0.0.0" | `ES_NODE_ARCHIVER_ADDRESS` |
| `--archiver.port <value>` | Blob archiver listening port | 9645 | `ES_NODE_ARCHIVER_PORT` |
| `--archiver.maxBlobsPerBlock <value>` | Max blobs per block | 0 | `ES_NODE_ARCHIVER_MAX_BLOBS_PER_BLOCK` |
| `--scanner.mode <value>` | Data scan mode, 0: disabled, 1: check meta, 2: check blob | 1 | `ES_NODE_SCANNER_MODE` |
| `--scanner.batch-size <value>` | Data scan batch size | 8192 | `ES_NODE_SCANNER_BATCH_SIZE` |
| `--scanner.interval <value>` | Data scan interval in minutes, minimum 3 (default) | 3 | `ES_NODE_SCANNER_INTERVAL` |
| `--email.username <value>` | Email username for notifications | N/A | `ES_NODE_EMAIL_USERNAME` |
| `--email.password <value>` | Email password for notifications | N/A | `ES_NODE_EMAIL_PASSWORD` |
| `--email.host <value>` | Email host for notifications | "smtp.gmail.com" | `ES_NODE_EMAIL_HOST` |
| `--email.port <value>` | Email port for notifications | 587 | `ES_NODE_EMAIL_PORT` |
| `--email.to <value>` | Email addresses to send notifications to | N/A | `ES_NODE_EMAIL_TO` |
| `--email.from <value>` | Email address that will appear as the sender of the notifications | N/A | `ES_NODE_EMAIL_FROM` |

