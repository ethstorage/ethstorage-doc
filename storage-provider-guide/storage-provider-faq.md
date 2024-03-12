# FAQ by Storage Providers


### What should I do when the log frequently shows "i/o deadline reached" during data syncing?

This problem is caused by the synchronization request timeout due to network reasons. It's possible that the amount of data requested is too large relative to network performance. Therefore, all requested data cannot be returned within the specified time.

You can change the size of each request to a smaller value using `--p2p.max.request.size`. 
The current value in the run.sh is `4194304`. You can try adjusting it to `1048576`.

### How to tune the performance of syncing? 

To enhance syncing performance or limit the usage of CPU power, you can adjust the values of the following related flags with higher or lower values:

- The `--p2p.sync.concurrency` flag determines the number of threads that simultaneously retrieve shard data, with a default setting of 16.
- The `--p2p.fill-empty.concurrency` flag specifies the number of threads used to concurrently fill encoded empty blobs, with a default setting of the CPU number minus 2.

### Why is the CPU fully utilized when syncing data? Does this imply that running an es-node requires high-end hardware? Additionally, is there a way to configure it to use less CPU power?

During data synchronization, the CPUs are fully utilized as the es-node is engaged in intensive data processing tasks like encoding and writing.

However, this high-intensity processing occurs primarily when large volumes of data need to be synchronized, such as during the initial startup of the es-node. Once the synchronization is complete, and the es-node enters the mining phase, the CPU usage decreases significantly. So, generaly speaking, the es-node does not require high-end hardware, particularly for the CPU.

If there is a need to conserve CPU power anyway, you can tune down the values of syncing performance related flags, namely `--p2p.sync.concurrency` and `--p2p.fill-empty.concurrency`. See [here](#how-to-tune-the-performance-of-syncing) for detailed information.

### What does it means when the log shows "The nonces are exhausted in this slot"

When you see "The nonces are exhausted in this slot...", it indicates that your node has successfully completed all the sampling tasks within a slot, and you do not need to do anything about it. 

### What can I do about "Mining tasks timed out"? 

When you see the message "Mining tasks timed out", it indicates that the sampling task couldn't be completed within a slot. 

You can check the IOPS of the disk to determine if the rate of data read has reached the IO capacity. 
If not, using the flag `--miner.threads-per-shard` can specify the number of threads to perform sampling for each shard, thereby helping in accomplishing additional sampling.

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

By default, when executed, the run.sh script will generate a data file named `shard-0.dat` to store shard 0 in the data directory specified by `--datadir`, located in the same folder as the run.sh script.

If necessary, you can choose an alternative location for data storage by specifying the full path of the file as the value of the `--storage.files` flag in the run.sh script.

Please refer to [configuration](/storage-provider-guide/configuration.md) for more details.

### What can I do about "The zkey file was not downloaded" error?

When you see the following message when running **run.sh**. you can manually download the [**blob_poseidon.zkey**](https://drive.google.com/file/d/1ZLfhYeCXMnbk6wUiBADRAn1mZ8MI_zg-/view) / [**blob_poseidon2.zkey**](https://drive.google.com/file/d/1olfJvXPJ25Rbcjj9udFlIVr08cUCgE4l/view) to `./build/bin/snarkjs/` folder and run it again. 
```
zk prover mode is 2
Start downloading ./build/bin/snarkjs/blob_poseidon2.zkey...
... ...
Error: The zkey file was not downloaded. Please try again.
```

### How can I change the default configurations?

Just append the flag and value to the end of command that execute `run.sh`.

Take `--p2p.max.request.size` as example, the following command change the default value from `4194304` in `run.sh` into `1048576`:

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
          ghcr.io/ethstorage/es-node:v0.1.9 \
          --p2p.max.request.size 1048576
```