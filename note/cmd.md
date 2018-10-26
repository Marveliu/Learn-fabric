# peer



```cmd
Usage:
  peer channel [command]

Available Commands:
  create       Create a channel
  fetch        Fetch a block
  getinfo      get blockchain information of a specified channel.
  join         Joins the peer to a channel.
  list         List of channels peer has joined.
  signconfigtx Signs a configtx update.
  update       Send a configtx update.

Flags:
      --cafile string                       Path to file containing PEM-encoded trusted certificate(s) for the ordering endpoint
      --certfile string                     Path to file containing PEM-encoded X509 public key to use for mutual TLS communication with the orderer endpoint
      --clientauth                          Use mutual TLS when communicating with the orderer endpoint
      --connTimeout duration                Timeout for client to connect (default 3s)
  -h, --help                                help for channel
      --keyfile string                      Path to file containing PEM-encoded private key to use for mutual TLS communication with the orderer endpoint
  -o, --orderer string                      Ordering service endpoint
      --ordererTLSHostnameOverride string   The hostname override to use when validating the TLS connection to the orderer.
      --tls                                 Use TLS when communicating with the orderer endpoint

Global Flags:
      --logging-level string   Default logging level and overrides, see core.yaml for full syntax

```

从共识节点拉第一个区块
peer channel fetch 0 0_block.pb -o orderer.example.com:7050 -c "$ORDERER_SYSCHAN_ID" --tls --cafile $ORDERER_CA >&log.txt