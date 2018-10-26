> Farbic配置文件以及相关工具


# 工具

## crytogen 

### usage

```cmd
cryptogen generate --config=./crypto-config.yaml
```

--config 指定yaml格式的配置文件，[click](#crypto-config.yaml)

```cmd
├── ordererOrganizations
│   └── example.com
└── peerOrganizations
    ├── org1.example.com
    └── org2.example.com
│       ├── ca          // 私钥以及pem编码的证书
│       ├── msp         // msp相关文件，Admin管理员证书，CA证书，TLS传输控制证书，皆为pem编码
│       ├── orderers    // 共识相关，msp同上，tls下面有CA认证后的证书文crt，server侧
│       ├── tlsca       // 传输控制协议的ca，同上ca
│       └── users       // 用户相关，同orders
```


## configtxgen

[源码地址](/common/tools/configtxgen)

> 初始化配置，并打包到交易块

```cmd
Usage of configtxgen:
  -asOrg string     // 申明确定的组织
        Performs the config generation as a particular organization (by name), only including values in the write set that org (likely) has privilege to set
  -channelID string // 申明channelID
        The channel ID to use in the configtx
  -configPath string    // 申明配置文件地址
        The path containing the configuration to use (if set)
  -inspectBlock string  // 打印出配置区块的相关信息到指定路径
        Prints the configuration contained in the block at the specified path
  -inspectChannelCreateTx string // 打印出配置通道信息到指定路径
        Prints the configuration contained in the transaction at the specified path
  -outputAnchorPeersUpdate string   // 输出锚节点更新配置
        Creates an config update to update an anchor peer (works only with the default channel creation, and only for the first update)
  -outputBlock string   // 输出初始配置区块
        The path to write the genesis block to (if set)
  -outputCreateChannelTx string // 输出通道交易
        The path to write a channel creation configtx to (if set)
  -printOrg string  // 打印出机构信息
        Prints the definition of an organization as JSON. (useful for adding an org to a channel manually)
  -profile string   // 申明配置文件中的profile
        The profile from configtx.yaml to use for generation. (default "SampleInsecureSolo")
  -version
        Show version information
```

## idemixgen 

> idemixgen is a command line tool that generates the CA's keys and generates MSP configs for siging and for verification，
This tool can be used to setup the peers and CA to support the Identity Mixer MSP

该工具用来生成CA's key以及生成MSP配置以便进行签名和验证。也可以用来初始化节点和节点用来支持混合身份模式的MSP.

todo: Identity Mixer MSP

```cmd
usage: idemixgen [<flags>] <command> [<args> ...]
Utility for generating key material to be used with the Identity Mixer MSP in Hyperledger Fabric

Flags:
  -h, --help                    Show context-sensitive help (also try --help-long and --help-man).
      --output="idemix-config"  The output directory in which to place artifacts

Commands:
  help [<command>...]
    Show help.

  ca-keygen
    Generate CA key material // 生成CA key相关的材料

  signerconfig [<flags>]
    Generate a default signer for this Idemix MSP    // 生成该Idemix的默认的签署人

  version
    Show version information

```


```cmd
idemix-config
├── ca                          // ca相关
│   ├── IssuerPublicKey         // 签署人公钥
│   ├── IssuerSecretKey         // 签署人私钥
│   └── RevocationKey           // 回收使用的私钥
└── msp                         // 同上
    ├── IssuerPublicKey         
    └── RevocationPublicKey
└── user
    └── SignerConfig            // signerconfig
```



# 配置文件


## crypto-config.yaml

```yaml
# 定义管理共识节点的组织
OrdererOrgs:
  - Name: Orderer
    Domain: example.com
    Specs:
      - Hostname: orderer

# 定义背书节点的组织
PeerOrgs:
  - Name: Org1
    Domain: org1.example.com
    EnableNodeOUs: true
    # ---------------------------------------------------------------------------
    # "Specs"
    # ---------------------------------------------------------------------------
    # Uncomment this section to enable the explicit definition of hosts in your
    # configuration.  Most users will want to use Template, below
    #
    # Specs is an array of Spec entries.  Each Spec entry consists of two fields:
    #   - Hostname:   (Required) The desired hostname, sans the domain.
    #   - CommonName: (Optional) Specifies the template or explicit override for
    #                 the CN.  By default, this is the template:
    #
    #                              "{{.Hostname}}.{{.Domain}}"
    #
    #                 which obtains its values from the Spec.Hostname and
    #                 Org.Domain, respectively.
    # ---------------------------------------------------------------------------
    # Specs:
    #   - Hostname: foo # implicitly "foo.org1.example.com"
    #     CommonName: foo27.org5.example.com # overrides Hostname-based FQDN set above
    #   - Hostname: bar
    #   - Hostname: baz
    # ---------------------------------------------------------------------------
    # "Template"
    # ---------------------------------------------------------------------------
    # Allows for the definition of 1 or more hosts that are created sequentially
    # from a template. By default, this looks like "peer%d" from 0 to Count-1.
    # You may override the number of nodes (Count), the starting index (Start)
    # or the template used to construct the name (Hostname).
    #
    # Note: Template and Specs are not mutually exclusive.  You may define both
    # sections and the aggregate nodes will be created for you.  Take care with
    # name collisions
    # ---------------------------------------------------------------------------
    Template:
      Count: 2
      # Start: 5
      # Hostname: {{.Prefix}}{{.Index}} # default
    # ---------------------------------------------------------------------------
    # "Users"
    # ---------------------------------------------------------------------------
    # Count: The number of user accounts _in addition_ to Admin
    # ---------------------------------------------------------------------------
    Users:
      Count: 1
  # ---------------------------------------------------------------------------
  # Org2: See "Org1" for full specification
  # ---------------------------------------------------------------------------
  - Name: Org2
    Domain: org2.example.com
    EnableNodeOUs: true
    Template:
      Count: 2
    Users:
      Count: 1

```

