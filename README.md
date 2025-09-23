# 🏛️ Blockchain-Based Land Registry System

A decentralized land registry system built on blockchain technology, providing secure, transparent, and immutable property ownership records.

![Land Registry](https://img.shields.io/badge/Blockchain-Land%20Registry-blue)
![Solidity](https://img.shields.io/badge/Solidity-0.8.0+-green)
![License](https://img.shields.io/badge/License-MIT-yellow)
![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)

## 🚀 **Quick Start**

### **Prerequisites**
- MetaMask wallet
- Test tokens for gas fees
- Modern web browser

### **Installation**
1. Clone the repository
2. Start local server:
   ```bash
   python -m http.server 8000
   # or
   npx http-server -p 8000
   ```
3. Open `http://localhost:8000`
4. Connect MetaMask wallet
5. Get test tokens from faucets

### **Demo Flow**
1. **Register** → Admin registers new properties
2. **Transfer** → Owners transfer property rights
3. **Verify** → Anyone can verify ownership

## ✨ **Features**

- 🔒 **Secure**: Cryptographic security through blockchain
- 🌐 **Transparent**: Public verification of ownership
- 📱 **Responsive**: Works on desktop and mobile
- 🔄 **Immutable**: Tamper-proof ownership records
- 💰 **Cost-Effective**: Low transaction fees
- 🚀 **Fast**: Automated smart contract execution

## 🏗️ **Architecture**

```
Frontend (HTML/JS) ↔ Smart Contract (Solidity) ↔ Blockchain (Polygon/ETH)
```

### **Technology Stack**
- **Frontend**: HTML5, CSS3, JavaScript ES6+, Viem.js
- **Backend**: Ethereum Smart Contracts (Solidity)
- **Blockchain**: Polygon Amoy & Ethereum Sepolia
- **Wallet**: MetaMask integration
- **Storage**: IPFS support (planned)

## 📋 **Core Functions**

### **Property Management**
- ✅ **Registration**: Admin-only property registration
- ✅ **Transfer**: Secure ownership transfers
- ✅ **Verification**: Public ownership verification
- ✅ **History**: Complete ownership chain tracking

### **Admin Controls**
- ✅ **Status Management**: Activate/deactivate properties
- ✅ **Metadata Updates**: Update property documentation
- ✅ **Access Control**: Role-based permissions

## 🌐 **Supported Networks**

| Network | Chain ID | Status | Faucet |
|---------|----------|--------|---------|
| Polygon Amoy | 80002 | ✅ Active | [Get Tokens](https://faucet.polygon.technology/) |
| Ethereum Sepolia | 11155111 | ✅ Active | [Get Tokens](https://sepoliafaucet.com) |

## 📖 **Documentation**

- 📋 [Project Report](PROJECT_REPORT.md) - Comprehensive project overview
- 👥 [User Guide](USER_GUIDE.md) - Step-by-step usage instructions
- 🔧 [Technical Docs](TECHNICAL_DOCS.md) - Developer documentation
- 🚀 [Deployment Guide](TECHNICAL_DOCS.md#deployment-guide) - Setup instructions

## 🎯 **Usage Examples**

### **Register Property**
```javascript
// Admin registers new property
await contract.registerProperty(
    "PLOT-001-DOWNTOWN",           // External ID
    "0x742d35Cc...",              // Owner address
    "ipfs://QmXx..."              // Metadata URI
);
```

### **Transfer Property**
```javascript
// Owner transfers property
await contract.transferProperty(
    "0x1234567890abcdef...",      // Property ID
    "0x9876543210fedcba...",      // New owner
    "Property sale 2025-01-15"    // Transfer note
);
```

### **Verify Ownership**
```javascript
// Anyone can verify
const property = await contract.getProperty(propertyId);
const history = await contract.getOwnershipHistory(propertyId);
```

## 🔧 **Development**

### **Local Development**
```bash
# Start development server
python -m http.server 8000

# Or use Node.js
npx http-server -p 8000

# Open browser
open http://localhost:8000
```

### **Smart Contract**
- **Contract Address**: `0x867c525d6b05b890a9aca148ab7ad048e6007342`
- **ABI**: Available in `abi.json`
- **Network**: Polygon Amoy & Sepolia

### **Testing**
```bash
# Run tests
npm test

# Manual testing checklist
- [ ] Wallet connection
- [ ] Property registration
- [ ] Property transfer
- [ ] Property verification
- [ ] Admin controls
```

## 🛡️ **Security**

### **Smart Contract Security**
- ✅ Access control implemented
- ✅ Input validation comprehensive
- ✅ Event logging complete
- ✅ Gas optimization efficient

### **Best Practices**
- 🔐 Never share private keys
- ✅ Always verify transaction details
- 🌐 Use trusted networks only
- 📱 Keep wallet software updated

## 📊 **Project Status**

| Component | Status | Version |
|-----------|--------|---------|
| Smart Contract | ✅ Deployed | 1.0.0 |
| Frontend | ✅ Complete | 1.0.0 |
| Documentation | ✅ Complete | 1.0.0 |
| Testing | ✅ Complete | 1.0.0 |

## 🚀 **Roadmap**

### **Phase 2** (Q2 2025)
- [ ] IPFS integration for document storage
- [ ] Multi-signature property ownership
- [ ] Property valuation integration
- [ ] Mobile application

### **Phase 3** (Q3 2025)
- [ ] Mainnet deployment
- [ ] Legal compliance framework
- [ ] API for third-party integration
- [ ] Advanced analytics dashboard

## 🤝 **Contributing**

We welcome contributions! Please see our contributing guidelines:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

## 📄 **License**

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 **Support**

- 📧 **Email**: support@landregistry.dev
- 💬 **Discord**: [Join our community](https://discord.gg/landregistry)
- 📚 **Documentation**: [Full docs](USER_GUIDE.md)
- 🐛 **Issues**: [Report bugs](https://github.com/landregistry/issues)

## 🙏 **Acknowledgments**

- Ethereum Foundation for blockchain infrastructure
- Polygon team for scaling solutions
- MetaMask for wallet integration
- Open source community for tools and libraries

---

**Made with ❤️ for secure, transparent land ownership**

*Last Updated: $(Get-Date)*
*Version: 1.0.0*

