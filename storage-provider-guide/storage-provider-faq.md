# FAQ by Storage Providers


### What should I do when the log frequently shows "i/o deadline reached" during data syncing?

This problem is often caused by a poor internet connection between your node and some peers, a common issue given that peers are located worldwide. Thus, it's likely you'll experience connection problems with some of them. The key detail to determine is the estimated syncing time shown in the log.
```
INFO [01-19|14:41:48.715] "Storage sync in progress" progress=14.46% peercount=21 tasksRemain=1@32 blobssynced=289,248@35.31GiB blobsTosync=1,711,107 timeused=2h42m13s etaTimeLeft=15h59m37s
```
If it's less than 24 hours, it should be acceptable. Additionally, it's possible that the data request size is too large compared to network performance, preventing all requested data from being returned in time.

You can change the size of each request to a smaller value using `--p2p.max.request.size`. 
The current value in the `run.sh` is `4194304`. You can try adjusting it to `1048576`. Check [here](#how-can-i-change-the-default-configurations) for the detailed operation.

### How to tune the performance of syncing? 

To enhance syncing performance or limit the usage of CPU power, you can adjust the values of the following related flags with higher or lower values:

- The `--p2p.sync.concurrency` flag determines the number of threads that simultaneously retrieve shard data, with a default setting of 16.
- The `--p2p.fill-empty.concurrency` flag specifies the number of threads used to concurrently fill encoded empty blobs, with a default setting of the CPU number minus 2.

Check [here](#how-can-i-change-the-default-configurations) for the detailed operation.

### Why is the CPU fully utilized when syncing data? Does this imply that running an es-node requires high-end hardware? Additionally, is there a way to configure it to use less CPU power?

During data synchronization, the CPUs are fully utilized as the es-node is engaged in intensive data processing tasks like encoding and writing.

However, this high-intensity processing occurs primarily when large volumes of data need to be synchronized, such as during the initial startup of the es-node. Once the synchronization is complete, and the es-node enters the mining phase, the CPU usage decreases significantly. So, generaly speaking, the es-node does not require high-end hardware, particularly for the CPU.

If there is a need to conserve CPU power anyway, you can tune down the values of syncing performance related flags, namely `--p2p.sync.concurrency` and `--p2p.fill-empty.concurrency`. See [here](#how-to-tune-the-performance-of-syncing) for detailed information.

### What does it means when the log shows "The nonces are exhausted in this slot"

When you see "The nonces are exhausted in this slot...", it indicates that your node has successfully completed all the sampling tasks within a slot, and you do not need to do anything about it. 

### What can I do about "Mining tasks timed out"? 

When you see the message "Mining tasks timed out", it indicates that the sampling task couldn't be completed within a slot. 

You can check the IOPS of the disk to determine if the rate of data read has reached the IO capacity. 
If not, using the flag `--miner.threads-per-shard` can specify the number of threads to perform sampling for each shard, thereby helping in accomplishing additional sampling. Check [here](#how-can-i-change-the-default-configurations) for the detailed operation.

If the previously mentioned method doesn't work, it's important to check if your NVMe disk supports PCIe 3.0 or 4.0 standards. Some PCIe 3.0 disks may not deliver sufficient random IOPS for the task. To ensure timely completion of the sampling process, upgrading to a PCIe 4.0 disk might be necessary.

### How do I know whether I've got a mining reward?

You can observe the logs to see if there are such messages indicating a successful storage proof submission:

```
INFO [01-19|14:41:48.715] Mining transaction confirmed             txHash=62df87..7dbfbc
INFO [01-19|14:41:49.017] "Mining transaction success!      √"     miner=0x534632D6d7aD1fe5f832951c97FDe73E4eFD9a77
INFO [01-19|14:41:49.018] Mining transaction details               txHash=62df87..7dbfbc gasUsed=419,892 effectiveGasPrice=2,000,000,009
INFO [01-19|14:41:49.018] Mining transaction accounting (in ether) reward=0.01013071059 cost=0.0008397840038 profit=0.009290926583

```

You can also visit [the EthStorage dashboard](http://grafana.ethstorage.io) for real-time mining statistics.

Finally, pay attention to the balance change of your miner's address which reflects the mining income.

### Can I change the data file location?

Yes, you can. 

By default, when executed, the `run.sh` script will generate a data file named `shard-0.dat` to store shard 0 in the data directory specified by `--datadir`, located in the same folder as the `run.sh` script.

If necessary, you can choose an alternative location for data storage by specifying the full path of the file as the value of the `--storage.files` flag in the `run.sh` script.

Please refer to [configuration](/storage-provider-guide/configuration.md) for more details.

### What can I do about "The zkey file was not downloaded" error?

When you see the following message when running **run.sh**. you can manually download the [**blob_poseidon.zkey**](https://drive.google.com/file/d/1ZLfhYeCXMnbk6wUiBADRAn1mZ8MI_zg-/view) / [**blob_poseidon2.zkey**](https://drive.google.com/file/d/1G7LmOx7hNE5GHc-M6yOjVB3ZZ4J6xUYO/view) to `./build/bin/snarkjs/` folder and run it again. 
```
zk prover mode is 2
Start downloading ./build/bin/snarkjs/blob_poseidon2.zkey...
... ...
Error: The zkey file was not downloaded. Please try again.
```

### How can I change the default configurations?

Just append the flag and value to the end of command that execute `run.sh`.

Take `--p2p.max.request.size` as example, the following command set its value to `1048576`:

```sh
env ES_NODE_STORAGE_MINER=<miner> ES_NODE_SIGNER_PRIVATE_KEY=<private_key> ./run.sh --p2p.max.request.size 1048576
```
If you start es-node with docker, please note that the flag and value should be added afer the docker image name:

```sh
docker run --name es  -d  \
          -v ./es-data:/es-node/es-data \
          -e ES_NODE_STORAGE_MINER=<miner> \
          -e ES_NODE_SIGNER_PRIVATE_KEY=<private_key> \
          -p 9545:9545 \
          -p 9222:9222 \
          -p 30305:30305/udp \
          --entrypoint /es-node/run.sh \
          ghcr.io/ethstorage/es-node:v0.1.10 \
          --p2p.max.request.size 1048576
```

### Why does the "missing trie node" error occur?


```
Save blobs error err="missing trie node c4b8803bb72e5841894b5348db64a818f2e6481e51bb35bb49f8eb41c7ba57a8 (path ) state Bxc4be883bb72e5841094b5340db64a818f2e6481e51bb35bb49f8eb41c7ba57a8 is not available"
```
If you encounter the `missing trie node` error as described, it is likely that your es-node attempted to access the L1 state older than 256 blocks without success. This may be due to your execution layer endpoint not being an archive node.

If you are using a `BlockPI` endpoint, please navigate to the dashboard, access the detailed page of your API endpoint, and enable the `Archive Mode` within the `Advanced Features` section.