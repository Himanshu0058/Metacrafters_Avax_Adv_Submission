# Building Your DeFi Empire: EVM Subnet & Smart Contracts on Avalanche

Welcome to this exciting challenge! In this tutorial, you'll learn how to set up your very own EVM subnet on Avalanche, which is an excellent starting point for building your DeFi Kingdom clone. We'll guide you through creating and deploying a custom subnet and then deploying two smart contracts onto it: an ERC20 token contract and a Vault contract. These contracts will serve as the foundational building blocks for your DeFi Kingdom clone, allowing you to explore the exciting world of decentralized finance and take your first steps towards building your very own DeFi empire. Let's get started!

Please note that in this discussion, we will not be delving into the intricacies of how DeFi works or how to generate value for our users. While it's important to understand how DeFi works and how to generate value for users, our current discussion is focused on setting up an EVM subnet on Avalanche and deploying foundational smart contracts for building a DeFi Kingdom clone. To stay on track, we'll keep our focus on these fundamentals and take our first steps towards creating a thriving DeFi empire.

## Steps Overview:

### 1. Set up your EVM subnet:
You can use our guide and the Avalanche documentation to create a custom EVM subnet on the Avalanche network.

### 2. Define your native currency:
Set up your own native currency, which can be used as the in-game currency for your DeFi Kingdom clone.

### 3. Connect to Metamask:
Connect your EVM Subnet to Metamask by following the steps laid out in our guide.

### 4. Deploy basic building blocks:
You can use Solidity and Remix to deploy the basic building blocks of your game, such as smart contracts for battling, exploring, and trading. These contracts will define the game rules, such as liquidity pools, tokens, and more.

#### a. ERC20 Contract Example:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    uint public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;
    string public name = "Solidity by Example";
    string public symbol = "SOLBYEX";
    uint8 public decimals = 18;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
```

#### b. Vault Contract Example:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Vault {
    IERC20 public immutable token;
    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function deposit(uint _amount) external {
        uint shares = totalSupply == 0 ? _amount : (_amount * totalSupply) / token.balanceOf(address(this));
        _mint(msg.sender, shares);
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        _burn(msg.sender, _shares);
        token.transfer(msg.sender, amount);
    }

    function _mint(address _to, uint _shares) private {
        totalSupply += _shares;
        balanceOf[_to] += _shares;
    }

    function _burn(address _from, uint _shares) private {
        totalSupply -= _shares;
        balanceOf[_from] -= _shares;
    }
}
```

With these steps, you can create a DeFi Kingdom clone on Avalanche that allows players to collect, build, and earn rewards for their participation in the game's activities.

## Tools Used:

- Unix Computer (MacOS or Linux)
- Solidity
- Remix
- Metamask
- Web Browser

## Your Project: Step by Step

1. Deploy your EVM subnet using the Avalanche CLI.
2. Add your Subnet to Metamask.
3. Make sure it is your selected network in Metamask.
4. Connect Remix to your Metamask using the Injected Provider.
5. Deploy the smart contracts.
6. Test your application!
