# simple-defi
一个简单的defi demo，可以质押 Dai token，也可以赎回；Owner还可以不定期给有质押的用户奖励Dapp token。

**步骤**

1. 下载代码，搭建开发环境：
    - 运行npm install，为项目添加依赖，包括truffle开发环境，前端React组件，web3js以及测试环境chai等，
    - Chrome浏览器安装metamask钱包插件，
    - 安装ganache客户端并启动本地节点，注意ganache本地节点的netwokid是1337，端口是7545，如果端口号变动需要在truffle-configure.js文件中更新；
2. 开启终端，cd到项目目录，运行truffle complie，为合约生成abi文件；
3. 运行truffle test可以运行测试；
4. 运行truffle migrate --reset，第一次部署可以省略--reset
5. 运行truffle console打开truffle控制台，可以键入web3命令来获取状态数据，比如：
    - accounts = await web3.eth.getAccounts(),获取全部账户，
    - balance = await web3.eth.getBalance(account)获取指定账户余额。
7. 运行truffle migrate部署合约，注意在migration文件夹下有自定义的2_deploy_contract.js，前面的序号代表部署的顺序，部署的时候会做这些预设：
    - 给TokenFarm合约transfer足够的dapp token，这些token用来不定期给质押的用户发奖励
    - 给第二个account发100个Dai Token，这个账号会用来实现质押和赎回的操作
8. 以上步骤完成合约部署，运行yarn start，启动react服务，浏览器呈现dapp页面
9. 当用户质押一定数额的Dai token之后，可以做命令行运行 truffle exec scripts/issue-token.js，给质押了Dai token的用户发相同数额的Dapp token奖励。

**要点**
1. Dai Token和Dapp Token都是ERC20合约，因为它们都实现了ERC20标准接口函数···contract ERC20 {
   function totalSupply() constant returns (uint theTotalSupply);
   function balanceOf(address _owner) constant returns (uint balance);
   function transfer(address _to, uint _value) returns (bool success);
   function transferFrom(address _from, address _to, uint _value) returns (bool success);
   function approve(address _spender, uint _value) returns (bool success);
   function allowance(address _owner, address _spender) constant returns (uint remaining);
   event Transfer(address indexed _from, address indexed _to, uint _value);
   event Approval(address indexed _owner, address indexed _spender, uint _value);
}···

2. Solidity emit event事件，比如Dai Token的Transfer事件，前端React如何监听，以更新DaiTokenBalance & stakingBalance？App.js中实现代码：···daiToken.events.Transfer(async (error, event) => {
        let daiTokenBalance = await daiToken.methods.balanceOf(this.state.account).call()
        this.setState({ daiTokenBalance: daiTokenBalance.toString() })
        const tokenFarm = new web3.eth.Contract(TokenFarm.abi, tokenFarmData.address)
        let stakingBalance = await tokenFarm.methods.stakingBalance(this.state.account).call()
        this.setState({ stakingBalance: stakingBalance.toString() })
      })···
