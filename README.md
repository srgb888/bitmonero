## Digital Cryptocurrency Blockchain Data_Cutting Bitmonero.Lmdb.Data.Mdb
### 数字加密货币区块区块链数据_剪裁 Bitmonero.Lmdb.Data.Mdb
```
种子文件 文件数：3个文件
Hash码: 6627f80ae4af815a687876f447ad287e9d067dc6
路径        size
data.mdb        41.60 GB
lock.mdb        8.06 KB
README.md        0.09 KB
```

### 使用 参考：   恢复数据库到哪里
在 Linux 上，默认安装 Monero，您可以在 ~/.bitmonero/lmdb/ 找到您的数据库。
如果要检查数据库的大小，可以使用以下命令
```
du -h ~/.bitmonero/lmdb/data.mdb
```
windows 使用GUI钱包，手动指定 区块链数据目录


## 如何在VPS上架设一个节点

本指南假定您已经设置了您的 VPS 帐户并正在使用 SSH 隧道连接到服务器控制台。
确保端口 18080 已打开
monerod 使用此端口与 Monero 网络上的其他节点进行通信。

例如，如果使用ufw：sudo ufw allow 18080 例如，如果使用iptables：sudo iptables -A INPUT -p tcp --dport 18080 -j ACCEPT
```
# 下载当前的 Monero Core 二进制文件
wget https://downloads.getmonero.org/linux64

# 创建一个目录并提取文件。
mkdir monero
tar -xjvf linux64 -C monero

# 启动守护进程
cd monero
./monerod

# 选项：将守护进程作为后台进程启动：
  ./monerod --detach

# 监控monerodif 作为守护进程运行的输出：
tail -f ~/.bitmonero/bitmonero.log
```

## 开启公网服务，搭建同步区块链远程节点
```
$ ./monerod  --detach  --rpc-bind-ip 0.0.0.0 --confirm-external-bind

2022-01-09 09:33:58.872	I Monero 'Oxygen Orion' (v0.17.3.0-release)
Forking to background...

$ ./monerod  status

2022-01-09 09:34:04.216	I Monero 'Oxygen Orion' (v0.17.3.0-release)
Height: 2533317/2533317 (100.0%) on mainnet, not mining, net hash 3.19 GH/s

$ ss -lt4

State    Local Address:Port 
LISTEN   0.0.0.0:18080
LISTEN   0.0.0.0:18081
```
## 客户端使用轻钱包,设置远程节点
- `set_daemon  IP或域名:18081 trusted`  再 `refresh` 刷新


# Monero 添加区块链修剪并提高交易效率

### 发布者：贾斯汀·埃伦霍夫 (Justin Ehrenhofer) 2019年2月1日

为了促进可扩展性，Monero 最近在其守护程序软件中添加了区块链修剪功能。此功能允许用户选择性地“修剪”大约 2/3 的区块链数据，同时仍然为网络做出贡献。在即将发布的 0.14 版本中还有其他几个效率升级。

## 什么是修剪？
修剪是从本地存储中删除非关键区块链信息的过程。完整节点保留存储在区块链上的所有内容的完整副本，包括不再有用的数据。修剪后的节点删除了大部分不太相关的信息，以减少占用空间。当然，运行全节点总是更好；然而，修剪后的节点拥有大部分重要信息，仍然可以支持网络。

对于比特币，很多人在中间交易的背景下讨论修剪。例如，假设 Alice 向 Bob 发送 1 BTC，然后 Bob 将其发送给 Charlie。区块链会记录交易 A -> B 和 B -> C 的记录。但是，由于 Alice 不能再花费她的资金，因此保留这些信息就不那么重要了。因此，节点可以以相对较高的安全级别修剪这些信息。如果发生恶意事件，网络上的其他节点将介入。

上面的例子不适用于门罗币，因为我们不知道钱什么时候花。然而，门罗节点可以修剪许多其他不必要的信息。这包括对防止双花不必要的环签名数据。虽然理论上 Monero 区块链可以比这个版本允许的更远，但需要更多的测试来推动这些限制。

## 修剪节省
门罗节点可以在仍然为网络做出贡献的同时修剪大量信息。修剪后的节点成功删除了整个区块链的大约 2/3。Monero 目前的区块链大约为 65GB。通过此更新，修剪后的节点只需要存储大约 25GB 的数据。

Monero 修剪节点只会修剪 7/8 的可修剪交易数据。保留随机 1/8 的数据。这 1/8 将用于与其他节点同步。修剪后的节点也将持有并共享最新的区块。

尽管修剪节点有助于 Monero 的安全性和去中心化，但它们仍然不如完整节点全面。用户仍应尽可能运行完整节点。但是，修剪节点比连接到其他人的远程节点要好。因此，修剪后的节点有可能在无法满足要求的设备上运行，从而减少使用远程节点的需要。

## 修剪门罗区块链的三种方法 [英文原文](https://www.publish0x.com/solareclipse/howto-prune-shrink-the-database-of-the-monero-blockchain-on-xpgwjx)
- 1.运行 `monerod` 使用 `--prune-blockchain` 参数
```
./monerod  --prune-blockchain
```

- 2.在 `monerod` 控制台中运行 `prune_blockchain`

- 3.运行 `monero-blockchain-prune` 实用程序
```
cd  ~/.bitmonero/lmdb
./monero-blockchain-prune
```


## 你的门罗数据库在哪里
在 Linux 上，默认安装门罗币，您可以在 `~/.bitmonero/lmdb` 找到您的数据库。
要验证修剪后的区块链的新大小，您可以再次使用相同的命令，但使用新数据库。

```
# 修剪前
du -h ~/.bitmonero/lmdb/data.mdb   #-> 80G

# 修剪后
du -h ~/.bitmonero/lmdb-pruned/data.mdb   #-> 26G
```
使用其区块链修剪 Monero 数据库为我节省了54G磁盘空间！哇！如果您从一开始就使用 SSD 驱动器，则可以在一夜之间完成这项工作。一旦数据库被修剪，它将在同步时保持修剪。无需再次修剪区块链。
