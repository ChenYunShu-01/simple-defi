# simple-defi
一个简单的defi demo，可以质押 Dai token，也可以赎回；Owner还可以不定期给有质押的用户奖励Dapp token。

**步骤**

1. 下载代码，搭建开发环境：
    - 运行npm install，为项目添加依赖，包括truffle开发环境，前端React组件，web3js以及测试环境chai等，
    - Chrome浏览器安装metamask钱包插件，
    - 安装ganache客户端并启动本地节点，注意ganache本地节点的netwokid是1337，端口是7545，如果端口号变动需要在truffle-configure.js文件中更新；
2. 开启终端，cd到项目目录，运行truffle complie，wei合约生成abi文件；
3. 运行truffle test可以运行测试；
4. 运行truffle migrate --reset，第一次部署可以省略--reset
5. 运行truffle console打开truffle控制台，可以键入web3命令来获取状态数据，比如accounts = await web3.eth.getAccounts(),获取全部账户，balance = await web3.eth.getBalance(account)获取指定账户余额。
6. 运行truffle migrate部署合约，
