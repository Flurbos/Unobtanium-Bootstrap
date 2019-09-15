# Unobtanium-Bootstrap
Construct a linear, no-fork, best version of the Unobtanium blockchain.
Ported from bitcoin Linearize.

## Step 1: Edit the .cfg
Before you begin creating a new bootstrap.dat you should ensure to setup the linearize.cfg, 
There is two example files available. The Blockbook example file is setup to practically run out 
of the box if you have followed jackie's blockbook tutorial and setup on a fresh debian 9 server.
The main things that will need editing is the `datadir`,`rpcusername`, `rpcpassword`, `max_height`, `input` and `output`
ensure the username and password match your daemon and datadir points to the location your unobtanium.conf file is located.
max_height is how many blocks you want added to the bootstrap.dat and the input and output is where your blocks folder is located and where you wish to output the bootstrap.dat. More info in the `example-linearize.cfg` file.
rename it to `linearize.cfg` when finished to continue the steps below.

## Step 2: Download hash list

    $ ./linearize-hashes.py linearize.cfg > hashlist.txt

Required configuration file settings for linearize-hashes:
* RPC: `datadir` (Required if `rpcuser` and `rpcpassword` are not specified)
* RPC: `rpcuser`, `rpcpassword` (Required if `datadir` is not specified)

Optional config file setting for linearize-hashes:
* RPC: `host`  (Default: `127.0.0.1`)
* RPC: `port`  (Default: `8332`)
* Blockchain: `min_height`, `max_height`
* `rev_hash_bytes`: If true, the written block hash list will be
byte-reversed. (In other words, the hash returned by getblockhash will have its
bytes reversed.) False by default. Intended for generation of
standalone hash lists but safe to use with linearize-data.py, which will output
the same data no matter which byte format is chosen.

The `linearize-hashes` script requires a connection, local or remote, to a
JSON-RPC server. Running `unobtaniumd` or `unobtanium-qt -server` will be sufficient.

## Step 3: Copy local block data

    $ ./linearize-data.py linearize.cfg

Required configuration file settings:
* `output_file`: The file that will contain the final blockchain.
      or
* `output`: Output directory for linearized `blocks/blkNNNNN.dat` output.

Optional config file setting for linearize-data:
* `debug_output`: Some printouts may not always be desired. If true, such output
will be printed.
* `file_timestamp`: Set each file's last-accessed and last-modified times,
respectively, to the current time and to the timestamp of the most recent block
written to the script's blockchain.
* `genesis`: The hash of the genesis block in the blockchain.
* `input`: unobtaniumd blocks/ directory containing blkNNNNN.dat
* `hashlist`: text file containing list of block hashes created by
linearize-hashes.py.
* `max_out_sz`: Maximum size for files created by the `output_file` option.
(Default: `1000*1000*1000 bytes`)
* `netmagic`: Network magic number.
* `out_of_order_cache_sz`: If out-of-order blocks are being read, the block can
be written to a cache so that the blockchain doesn't have to be sought again.
This option specifies the cache size. (Default: `100*1000*1000 bytes`)
* `rev_hash_bytes`: If true, the block hash list written by linearize-hashes.py
will be byte-reversed when read by linearize-data.py. See the linearize-hashes
entry for more information.
* `split_timestamp`: Split blockchain files when a new month is first seen, in
addition to reaching a maximum file size (`max_out_sz`).
