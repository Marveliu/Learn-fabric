> 创建Fabric网络场景

# [e2e-cli](examples/e2e_cli)

## 配置生成


generateCerts：生成机构的证书
generateIdemixMaterial: 生成MSP相关文件
replacePrivateKey: Using docker-compose template replace private key file names with constants
generateChannelArtifacts: 创建通道相关文件


docker-compose -f docker-compose-cli.yaml up
docker logs -f cli


examples/e2e_cli/base/docker-compose-base.yaml
定义了Zookeeper等许多服务

examples/e2e_cli/base/peer-base.yaml
定义peer容器

script.sh





