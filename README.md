# 🌐 Chainlink Bootcamp 2024 Token (`CLBoot24`)

An ERC-20 token built during the Chainlink Bootcamp 2024 with role-based access control for secure minting. This token includes a custom decimal setting and follows best practices using OpenZeppelin's audited contracts.

---

## 📆 Contract Summary

- **Name:** Chainlink Bootcamp 2024 Token  
- **Symbol:** `CLBoot24`  
- **Standard:** ERC-20  
- **Decimals:** 2 (customized)  
- **Roles:**  
  - `DEFAULT_ADMIN_ROLE`: Full administrative access  
  - `MINTER_ROLE`: Can mint new tokens

---

## 🛡️ Features

- ✅ ERC-20 standard compliant  
- ✅ Secure minting via `MINTER_ROLE`  
- ✅ Admin-controlled access to minting  
- ✅ Custom `decimals()` override to 2  
- ✅ Built with OpenZeppelin Contracts v4.6.0

---

## 🔍 Smart Contract Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

import "@openzeppelin/contracts@4.6.0/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts@4.6.0/access/AccessControl.sol";

contract Token is ERC20, AccessControl {
    bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");

    constructor() ERC20("Chainlink Bootcamp 2024 Token", "CLBoot24") {
        _grantRole(DEFAULT_ADMIN_ROLE, msg.sender);
        _grantRole(MINTER_ROLE, msg.sender);
    }

    function mint(address to, uint256 amount) public onlyRole(MINTER_ROLE) {
        _mint(to, amount);
    }

    function decimals() public pure override returns (uint8) {
        return 2;
    }
}
```

---

## 🚧 Deployment Using Foundry

### 1. Install Foundry
```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
```

### 2. Initialize Your Project
```bash
forge init clboot24-token
cd clboot24-token
```

### 3. Install OpenZeppelin Contracts
```bash
forge install OpenZeppelin/openzeppelin-contracts@v4.6.0
```

### 4. Add Your Contract to `src/Token.sol` and Compile
```bash
forge build
```

### 5. Create a Deployment Script
`script/Deploy.s.sol`:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "forge-std/Script.sol";
import "../src/Token.sol";

contract Deploy is Script {
    function run() external {
        vm.startBroadcast();
        new Token();
        vm.stopBroadcast();
    }
}
```

### 6. Deploy to a Network (e.g., Goerli)
```bash
forge script script/Deploy.s.sol:Deploy --rpc-url $GOERLI_RPC --private-key $PRIVATE_KEY --broadcast
```

---

## 💡 Suggested Improvements
To fully support external minting, you can expose this mint function:
```solidity
function mint(address to, uint256 amount) external onlyRole(MINTER_ROLE) {
    _mint(to, amount);
}
```

---

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## 🙌 Acknowledgements

- Built with 💙 at the [Chainlink Bootcamp 2024](https://chain.link/)  
- Powered by [OpenZeppelin Contracts](https://openzeppelin.com/contracts/)
