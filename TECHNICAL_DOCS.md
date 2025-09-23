# 🔧 Technical Documentation - Land Registry System

## 📋 **Table of Contents**
1. [System Architecture](#system-architecture)
2. [Smart Contract Details](#smart-contract-details)
3. [Frontend Implementation](#frontend-implementation)
4. [API Reference](#api-reference)
5. [Deployment Guide](#deployment-guide)
6. [Testing Procedures](#testing-procedures)
7. [Security Considerations](#security-considerations)

---

## 🏗️ **System Architecture**

### **High-Level Architecture**
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   Smart         │    │   Blockchain    │
│   (HTML/JS)     │◄──►│   Contract      │◄──►│   Network       │
│                 │    │   (Solidity)    │    │   (Polygon/ETH) │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   MetaMask      │    │   IPFS          │    │   Block         │
│   Wallet        │    │   Storage       │    │   Explorer      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### **Technology Stack**
- **Frontend**: HTML5, CSS3, JavaScript ES6+, Viem.js
- **Blockchain**: Ethereum Virtual Machine (EVM)
- **Smart Contract**: Solidity 0.8.0+
- **Networks**: Polygon Amoy, Ethereum Sepolia
- **Wallet**: MetaMask integration
- **Storage**: IPFS (planned)

---

## 📜 **Smart Contract Details**

### **Contract Information**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract LandRegistry {
    // Contract state variables
    address public admin;
    mapping(bytes32 => Property) public properties;
    mapping(string => bool) public externalIdsUsed;
    mapping(bytes32 => OwnershipEvent[]) public ownershipHistory;
    
    // Events
    event PropertyRegistered(bytes32 indexed propertyId, string externalId, address indexed owner, string metadataUri);
    event PropertyTransferred(bytes32 indexed propertyId, address indexed from, address indexed to, string note);
    event PropertyActivated(bytes32 indexed propertyId, bool active);
    event MetadataUpdated(bytes32 indexed propertyId, string newUri);
    event AdminChanged(address indexed previousAdmin, address indexed newAdmin);
}
```

### **Data Structures**

#### **Property Struct**
```solidity
struct Property {
    bytes32 propertyId;     // Unique blockchain identifier
    string metadataUri;     // IPFS link to documents
    address currentOwner;   // Current property owner
    bool active;           // Property status
    uint256 createdAt;     // Registration timestamp
    uint256 updatedAt;     // Last update timestamp
}
```

#### **OwnershipEvent Struct**
```solidity
struct OwnershipEvent {
    address from;          // Previous owner
    address to;            // New owner
    uint256 timestamp;     // Transfer timestamp
    string note;           // Transfer reason/note
}
```

### **Core Functions**

#### **Property Registration**
```solidity
function registerProperty(
    string memory externalId,
    address owner,
    string memory metadataUri
) public onlyAdmin returns (bytes32) {
    require(!externalIdsUsed[externalId], "External ID already used");
    
    bytes32 propertyId = keccak256(abi.encodePacked(externalId));
    properties[propertyId] = Property({
        propertyId: propertyId,
        metadataUri: metadataUri,
        currentOwner: owner,
        active: true,
        createdAt: block.timestamp,
        updatedAt: block.timestamp
    });
    
    externalIdsUsed[externalId] = true;
    emit PropertyRegistered(propertyId, externalId, owner, metadataUri);
    return propertyId;
}
```

#### **Property Transfer**
```solidity
function transferProperty(
    bytes32 propertyId,
    address newOwner,
    string memory note
) public {
    Property storage property = properties[propertyId];
    require(property.currentOwner == msg.sender, "Not the owner");
    require(property.active, "Property is not active");
    
    address previousOwner = property.currentOwner;
    property.currentOwner = newOwner;
    property.updatedAt = block.timestamp;
    
    ownershipHistory[propertyId].push(OwnershipEvent({
        from: previousOwner,
        to: newOwner,
        timestamp: block.timestamp,
        note: note
    }));
    
    emit PropertyTransferred(propertyId, previousOwner, newOwner, note);
}
```

#### **Property Verification**
```solidity
function getProperty(bytes32 propertyId) public view returns (Property memory) {
    return properties[propertyId];
}

function getOwnershipHistory(bytes32 propertyId) public view returns (OwnershipEvent[] memory) {
    return ownershipHistory[propertyId];
}
```

### **Access Control**
```solidity
modifier onlyAdmin() {
    require(msg.sender == admin, "Only admin can perform this action");
    _;
}

function setAdmin(address newAdmin) public onlyAdmin {
    address previousAdmin = admin;
    admin = newAdmin;
    emit AdminChanged(previousAdmin, newAdmin);
}
```

---

## 🎨 **Frontend Implementation**

### **Technology Stack**
- **Viem.js**: Ethereum library for interaction
- **ES6 Modules**: Modern JavaScript module system
- **CSS Grid/Flexbox**: Responsive layout system
- **Glassmorphism**: Modern UI design pattern

### **Key Components**

#### **Wallet Connection**
```javascript
async function connect() {
    try {
        const [account] = await provider.request({ method: "eth_requestAccounts" });
        const bal = await publicClient.getBalance({ address: account });
        $("connect").textContent = "Connected";
        $("connect").disabled = true;
        toast(`Connected: ${account.slice(0,6)}…${account.slice(-4)} — ${formatEther(bal)} ETH/MATIC`);
        return account;
    } catch (e) {
        toast(e.message || "Failed to connect", "error");
        throw e;
    }
}
```

#### **Contract Interaction**
```javascript
async function write(fn, args, btn, successMsg) {
    const account = await connect();
    try{
        busy(btn, true);
        const hash = await walletClient.writeContract({ 
            address: CONTRACT_ADDRESS, 
            abi: ABI, 
            functionName: fn, 
            args, 
            account 
        });
        toast("Txn sent, waiting…");
        const receipt = await publicClient.waitForTransactionReceipt({ hash });
        toast(successMsg || "Transaction confirmed");
        return { hash, receipt };
    } finally {
        busy(btn, false);
    }
}
```

#### **Property ID Generation**
```javascript
$("deriveId").onclick = () => {
    try{
        const externalId = $("externalId").value.trim();
        if (!externalId) throw new Error("Enter an externalId");
        const pid = keccak256(toHex(externalId));
        out("derivedId", pid);
        toast("Computed propertyId");
    }catch(e){ 
        out("derivedId", e.message); 
        toast(e.message, "error"); 
    }
};
```

### **UI Components**

#### **Responsive Grid Layout**
```css
.grid{ 
    display:grid; 
    gap:18px; 
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); 
}
```

#### **Glassmorphism Design**
```css
.card{
    background: var(--panel);
    border:1px solid rgba(255,255,255,.10);
    border-radius: var(--radius);
    box-shadow: var(--shadow);
    padding:16px;
}
```

#### **Toast Notifications**
```javascript
function toast(message, type="success"){
    const wrap = $("toasts");
    const el = document.createElement("div");
    el.className = `toast ${type}`;
    el.textContent = message;
    wrap.appendChild(el);
    setTimeout(() => el.remove(), 3500);
}
```

---

## 🔌 **API Reference**

### **Smart Contract Functions**

#### **Read Functions**
| Function | Parameters | Returns | Description |
|----------|------------|---------|-------------|
| `getProperty` | `bytes32 propertyId` | `Property` | Get property details |
| `getOwnershipHistory` | `bytes32 propertyId` | `OwnershipEvent[]` | Get ownership history |
| `isExternalIdUsed` | `string externalId` | `bool` | Check if external ID exists |
| `propertyIdFromExternalId` | `string externalId` | `bytes32` | Convert external ID to property ID |
| `admin` | None | `address` | Get admin address |

#### **Write Functions**
| Function | Parameters | Description | Access |
|----------|------------|-------------|---------|
| `registerProperty` | `string externalId, address owner, string metadataUri` | Register new property | Admin only |
| `transferProperty` | `bytes32 propertyId, address newOwner, string note` | Transfer property | Owner only |
| `setPropertyActive` | `bytes32 propertyId, bool active` | Set property status | Admin only |
| `updateMetadata` | `bytes32 propertyId, string newUri` | Update metadata | Admin only |
| `setAdmin` | `address newAdmin` | Change admin | Admin only |

### **Events**
| Event | Parameters | Description |
|-------|------------|-------------|
| `PropertyRegistered` | `bytes32 propertyId, string externalId, address owner, string metadataUri` | Property registered |
| `PropertyTransferred` | `bytes32 propertyId, address from, address to, string note` | Property transferred |
| `PropertyActivated` | `bytes32 propertyId, bool active` | Property status changed |
| `MetadataUpdated` | `bytes32 propertyId, string newUri` | Metadata updated |
| `AdminChanged` | `address previousAdmin, address newAdmin` | Admin changed |

---

## 🚀 **Deployment Guide**

### **Prerequisites**
- Node.js and npm installed
- MetaMask wallet with testnet tokens
- Remix IDE account (optional)

### **Smart Contract Deployment**

#### **Using Remix IDE**
1. Open https://remix.ethereum.org
2. Create new file: `LandRegistry.sol`
3. Paste contract code
4. Compile with Solidity 0.8.0+
5. Deploy to testnet
6. Copy contract address and ABI

#### **Using Hardhat/Truffle**
```bash
# Install dependencies
npm install --save-dev hardhat @nomiclabs/hardhat-ethers ethers

# Deploy contract
npx hardhat run scripts/deploy.js --network amoy
```

### **Frontend Deployment**

#### **Local Development**
```bash
# Start local server
python -m http.server 8000
# or
npx http-server -p 8000
```

#### **Production Deployment**
```bash
# Build for production
npm run build

# Deploy to web server
rsync -av dist/ user@server:/var/www/landregistry/
```

### **Environment Configuration**
```javascript
// config.js
export const CONFIG = {
    CONTRACT_ADDRESS: "0x867c525d6b05b890a9aca148ab7ad048e6007342",
    NETWORKS: {
        polygonAmoy: {
            chainId: 80002,
            rpcUrl: "https://rpc-amoy.polygon.technology"
        },
        sepolia: {
            chainId: 11155111,
            rpcUrl: "https://sepolia.infura.io/v3/YOUR_KEY"
        }
    }
};
```

---

## 🧪 **Testing Procedures**

### **Unit Testing**
```javascript
// test.js
import { describe, it, expect } from 'vitest';
import { LandRegistry } from './contracts/LandRegistry.sol';

describe('LandRegistry', () => {
    it('should register property', async () => {
        const contract = await LandRegistry.deploy();
        const tx = await contract.registerProperty("TEST-001", owner, "ipfs://...");
        expect(tx).to.be.ok;
    });
});
```

### **Integration Testing**
```javascript
// integration.test.js
describe('Frontend Integration', () => {
    it('should connect wallet', async () => {
        const result = await connectWallet();
        expect(result).to.have.property('address');
    });
    
    it('should register property', async () => {
        const result = await registerProperty("TEST-002", owner, metadata);
        expect(result).to.have.property('hash');
    });
});
```

### **Manual Testing Checklist**
- [ ] Wallet connection works
- [ ] Property registration functions
- [ ] Property transfer works
- [ ] Property verification accurate
- [ ] Admin controls function
- [ ] Error handling works
- [ ] UI responsive on mobile
- [ ] Gas estimation accurate

---

## 🛡️ **Security Considerations**

### **Smart Contract Security**

#### **Access Control**
- Admin-only functions properly protected
- Owner-only functions verified
- Input validation comprehensive

#### **Common Vulnerabilities**
- **Reentrancy**: Not applicable (no external calls)
- **Integer Overflow**: Protected by Solidity 0.8.0+
- **Front-running**: Considered in design
- **Gas Limit**: Optimized for efficiency

#### **Security Best Practices**
```solidity
// Input validation
require(bytes(externalId).length > 0, "External ID cannot be empty");
require(owner != address(0), "Invalid owner address");

// Access control
modifier onlyAdmin() {
    require(msg.sender == admin, "Only admin can perform this action");
    _;
}

// Event emission for transparency
emit PropertyRegistered(propertyId, externalId, owner, metadataUri);
```

### **Frontend Security**

#### **Input Sanitization**
```javascript
function parseBytes32(input) {
    const v = input.trim();
    if (!v.startsWith("0x") || v.length !== 66) {
        throw new Error("propertyId must be 0x + 64 hex chars");
    }
    return v;
}
```

#### **Error Handling**
```javascript
try {
    const result = await contractFunction();
    toast("Success", "success");
} catch (e) {
    toast(e.message || "Transaction failed", "error");
}
```

### **Network Security**
- Use only trusted RPC endpoints
- Verify contract addresses
- Check network configurations
- Monitor for suspicious activity

---

## 📊 **Performance Optimization**

### **Gas Optimization**
- Efficient storage patterns
- Minimal external calls
- Optimized data structures
- Batch operations where possible

### **Frontend Optimization**
- Lazy loading of components
- Efficient state management
- Optimized bundle size
- Caching strategies

### **Network Optimization**
- Connection pooling
- Request batching
- Error retry logic
- Offline support

---

## 🔄 **Maintenance & Updates**

### **Regular Maintenance**
- Monitor contract events
- Update dependencies
- Security audits
- Performance monitoring

### **Update Procedures**
- Test on testnet first
- Gradual rollout strategy
- Rollback procedures
- User notification system

### **Monitoring**
- Transaction success rates
- Gas usage patterns
- Error frequency
- User activity metrics

---

*Last Updated: $(Get-Date)*
*Version: 1.0.0*
*Technical Docs Version: 1.0.0*

