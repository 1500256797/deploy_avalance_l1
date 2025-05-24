# Avalanche L1 部署指南

## 0x1 前提条件

-   尽量支持 Linux 和 Mac 操作系统
-   需要安装 curl
-   官方文档：https://build.avax.network/docs/tooling/get-avalanche-cli
-   视频部署教程：https://www.bilibili.com/video/BV1SVj4zjETq/?vd_source=e997d3821c84d07d27f63407579f2f97

## 0x2 安装 Avalanche CLI

通过以下命令下载并安装最新版本的 Avalanche CLI:

```bash
curl -sSfL https://raw.githubusercontent.com/ava-labs/avalanche-cli/main/scripts/install.sh | sh -s
```

安装程序会将二进制文件安装到`~/bin`目录下。如果该目录不存在，将会自动创建。

## 0x3 将 Avalanche CLI 添加到 PATH

要从任何位置调用`avalanche`命令，需要将其添加到系统 PATH 中。如果使用默认位置安装，可以运行：

```
export PATH=~/bin:$PATH
```

要永久添加到 PATH，将上述命令添加到 shell 初始化脚本中：

-   对于 bash 用户，添加到`.bashrc`
-   对于 zsh 用户，添加到`.zshrc`

例如：

```
echo 'export PATH=~/bin:$PATH' >> .bashrc
```

## 0x4 验证安装

通过运行以下命令测试安装：

```
avalanche -v
```

## 0x5 创建并部署自定义 L1 链

### 1. 创建区块链

使用以下命令创建区块链配置：

```
avalanche blockchain create <L1名称>
```

例如：

```
avalanche blockchain create OHChain
```

在交互式提示中，您需要：

1. 选择 VM 类型（示例中选择了 Subnet-EVM）
2. 选择共识机制（示例中选择了 Proof Of Authority）
3. 选择 ValidatorManager 合约控制器地址（示例中从已存储的密钥中获取，选择了 ewoq）
4. 选择环境配置（示例中选择了测试环境默认配置）

创建过程会设置：

-   Chain ID（示例中为 150025）
-   Token 符号（示例中为 OH）
-   初始资金账户及余额

### 2. 部署区块链

创建配置后，使用以下命令部署区块链：

```
avalanche blockchain deploy <L1名称>
```

例如：

```
avalanche blockchain deploy OHChain
```

部署过程会：

1. 启动本地网络（如果尚未运行）
2. 等待区块链引导完成
3. 初始化验证器管理合约
4. 部署 ICM（跨链消息）相关合约
5. 生成和启动中继器
6. 显示详细的部署信息

## 0x6 部署后的信息

部署成功后，系统会显示大量信息，包括：

### 1. 区块链基本信息

-   区块链名称、VM ID、版本、验证机制
-   Chain ID、Subnet ID、所有者信息
-   RPC 端点

### 2. ICM（跨链消息）信息

-   ICM Messenger 地址
-   ICM Registry 地址

### 3. 代币信息

-   代币名称和符号

### 4. 初始代币分配

-   主要资金账户地址、私钥和余额
-   ICM 使用的账户地址、私钥和余额

### 5. 智能合约信息

-   验证器消息库地址
-   代理管理员地址
-   PoA 验证器管理器地址
-   透明代理地址

### 6. 预编译配置

-   Warp 预编译的管理员地址、管理器地址和启用地址信息

### 7. RPC URL

-   本地主机 RPC URL

### 8. 节点信息

-   主节点 ID 和端点
-   L1 节点 ID 和端点

### 9. 钱包连接信息

-   网络 RPC URL
-   网络名称
-   Chain ID
-   代币符号和名称

## 0x7 使用初始账户

部署完成后，您可以使用创建的初始账户：

主资金账户：

-   地址：0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC
-   私钥：56289e99c94b6912bfc12adc093c9b51124f0dc54ac7a766b2bc5ccf558d8027
-   余额：1,000,000 OH

ICM 使用账户：

-   地址：0x8f65BE1AF764997c91B39C673350fC58402156BA
-   私钥：fbbdfab222bd5b36c6455b18a2f6e3e8294ff4409e44d519e6ec40782d82cf07
-   余额：600 OH

您可以将这些账户导入到兼容的钱包（如 OKX 钱包）中使用。如果在钱包中看不到代币，可能需要重启网络。

## 0x8 管理网络

-   停止本地网络：`avalanche network stop`
-   重启本地网络：`avalanche network start`
-   清理网络：`avalanche network clean`

## 常见问题解答

**Q: 如何定制原生 token 的释放、增发机制等？**

A: 这与 token 发行在 L1 上还是其他链上关系不大。如果您在自己的 L1 上发行 token，那么 token 的详细细节、数量、释放机制等都在您的 Token Generation Contract 中实现。

**Q: Avalanche 上的 SVM(Subnet Virtual Machine)成熟度如何？**

A: 目前 Avalanche 上的 SVM 还不够成熟，官方希望社区参与开发。RWA(Real World Assets)也是一个发展方向。

**Q: 有哪些潜在应用方向？**

A: RWA+IOT 是一个有前景的方向：

-   IOT 产生价值，RWA 进行量化和打分
-   用户使用 IOT 服务需要通过 TOKEN 购买
-   IOT 可以是 PC 上的 docker 程序或手机上的 app（注意力经济）
