# 📖 Land Registry System - User Guide

## 🚀 **Quick Start Guide**

### **Prerequisites**
- MetaMask wallet installed
- Test tokens for gas fees
- Modern web browser (Chrome, Firefox, Edge)

### **Step 1: Setup MetaMask**
1. Install MetaMask browser extension
2. Create or import a wallet
3. Add testnet networks:
   - **Polygon Amoy**: Chain ID 80002
   - **Ethereum Sepolia**: Chain ID 11155111

### **Step 2: Get Test Tokens**
- **Polygon Amoy**: https://faucet.polygon.technology/
- **Ethereum Sepolia**: https://sepoliafaucet.com/

### **Step 3: Access the System**
1. Open your browser to `http://localhost:8000`
2. Click "Connect Wallet" button
3. Approve MetaMask connection

---

## 🏠 **Property Registration (Admin Only)**

### **Who Can Register Properties?**
Only the contract administrator can register new properties. The admin address is set during contract deployment.

### **How to Register a Property**

1. **Enter Property Details**:
   - **External ID**: Human-readable identifier (e.g., "PLOT-123-XYZ")
   - **Owner Address**: Ethereum address of the property owner
   - **Metadata URI**: IPFS link or URL to property documents

2. **Click "Register"**:
   - MetaMask will prompt for transaction approval
   - Pay gas fees to complete registration
   - Property ID will be generated automatically

3. **Verification**:
   - Transaction receipt confirms successful registration
   - Property appears in the blockchain registry
   - Ownership is immediately verifiable

### **Example Registration**
```
External ID: "PLOT-001-DOWNTOWN"
Owner: 0x742d35Cc6634C0532925a3b8D0C4b4b4b4b4b4b4
Metadata: "ipfs://QmXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxXx"
```

---

## 🔄 **Property Transfer**

### **Who Can Transfer Properties?**
Only the current property owner can transfer ownership to another address.

### **How to Transfer a Property**

1. **Get Property ID**:
   - Use "Derive Property ID" tool with external ID
   - Or copy from previous transaction receipt

2. **Enter Transfer Details**:
   - **Property ID**: 64-character hexadecimal ID (0x...)
   - **New Owner**: Ethereum address of the recipient
   - **Transfer Note**: Optional reason (e.g., "Sale 2025-01-15")

3. **Execute Transfer**:
   - Click "Transfer" button
   - Approve transaction in MetaMask
   - Pay gas fees

4. **Confirmation**:
   - Transfer is recorded on blockchain
   - New owner immediately gains control
   - Previous owner loses access

### **Transfer Example**
```
Property ID: 0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef
New Owner: 0x9876543210fedcba9876543210fedcba9876543210fedcba9876543210fedcba
Note: "Property sale to new owner"
```

---

## 🔍 **Property Verification**

### **Public Verification**
Anyone can verify property ownership and history without needing special permissions.

### **How to Verify a Property**

1. **Enter Property ID**:
   - Use the 64-character hexadecimal ID
   - Can be obtained from registration or transfer receipts

2. **Choose Verification Type**:
   - **Get Property**: View current ownership and details
   - **Get History**: View complete ownership chain

3. **View Results**:
   - Current owner address
   - Property status (active/inactive)
   - Creation and update timestamps
   - Complete ownership history

### **Verification Results Example**
```json
{
  "propertyId": "0x1234...",
  "metadataUri": "ipfs://QmXx...",
  "currentOwner": "0x742d...",
  "active": true,
  "createdAt": "1704067200",
  "updatedAt": "1704067200"
}
```

---

## 🛠️ **Admin Controls**

### **Admin Functions**
The contract administrator has additional capabilities for system management.

### **Set Property Status**
1. **Enter Property ID**
2. **Select Status**:
   - `true`: Activate property
   - `false`: Deactivate property
3. **Click "Set Active"**

### **Update Metadata**
1. **Enter Property ID**
2. **Enter New Metadata URI**:
   - IPFS link: `ipfs://QmXx...`
   - HTTP URL: `https://example.com/docs`
3. **Click "Update Metadata"**

---

## 🔧 **Property ID Generation**

### **Understanding Property IDs**
- **External ID**: Human-readable identifier (e.g., "PLOT-123")
- **Property ID**: Blockchain identifier (64-character hex)

### **How to Generate Property ID**
1. **Enter External ID**: Type your property identifier
2. **Click "Compute"**: System calculates the blockchain ID
3. **Copy Result**: Use this ID for all blockchain operations

### **Example**
```
External ID: "PLOT-123-XYZ"
Property ID: 0xabcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890
```

---

## 💰 **Gas Fees & Costs**

### **Understanding Gas Fees**
- **Gas**: Computational cost for blockchain operations
- **Gas Price**: Cost per unit of computation
- **Total Cost**: Gas × Gas Price

### **Typical Costs (Testnet)**
- **Property Registration**: ~0.001 ETH/MATIC
- **Property Transfer**: ~0.0005 ETH/MATIC
- **Property Verification**: Free (read-only)
- **Admin Functions**: ~0.0003 ETH/MATIC

### **Gas Optimization Tips**
- Use testnet for development
- Check gas prices before transactions
- Use MetaMask's gas estimation
- Consider gas price fluctuations

---

## 🚨 **Troubleshooting**

### **Common Issues & Solutions**

#### **"Contract not found" Error**
- **Cause**: Wrong network or contract not deployed
- **Solution**: Switch to correct testnet (Polygon Amoy/Sepolia)

#### **"Insufficient funds" Error**
- **Cause**: Not enough tokens for gas fees
- **Solution**: Get test tokens from faucet

#### **"Transaction failed" Error**
- **Cause**: Gas limit too low or network congestion
- **Solution**: Increase gas limit in MetaMask

#### **"Property not found" Error**
- **Cause**: Invalid property ID or property doesn't exist
- **Solution**: Verify property ID is correct

#### **"Access denied" Error**
- **Cause**: Not the property owner or admin
- **Solution**: Check if you have the required permissions

### **MetaMask Issues**
- **Connection Problems**: Refresh page and reconnect
- **Network Issues**: Switch networks in MetaMask
- **Transaction Stuck**: Cancel and retry with higher gas

---

## 📱 **Mobile Usage**

### **Mobile MetaMask**
1. Install MetaMask mobile app
2. Connect to same wallet as desktop
3. Use "WalletConnect" or "Mobile Browser"

### **Mobile Browser**
- Open mobile browser
- Navigate to `http://localhost:8000`
- Connect MetaMask mobile wallet
- Use touch-friendly interface

---

## 🔐 **Security Best Practices**

### **Wallet Security**
- **Never share private keys**
- **Use hardware wallets for large amounts**
- **Enable 2FA where possible**
- **Regular backup of wallet data**

### **Transaction Security**
- **Always verify transaction details**
- **Check recipient addresses carefully**
- **Use trusted networks only**
- **Be cautious of phishing sites**

### **Property Security**
- **Keep property IDs secure**
- **Verify ownership before transfers**
- **Use official channels only**
- **Report suspicious activity**

---

## 📞 **Support & Help**

### **Getting Help**
- **Documentation**: Check this guide first
- **Community**: Join developer forums
- **Technical Support**: Contact development team
- **Bug Reports**: Submit detailed issue reports

### **Useful Resources**
- **MetaMask Docs**: https://docs.metamask.io/
- **Polygon Docs**: https://docs.polygon.technology/
- **Ethereum Docs**: https://ethereum.org/developers/
- **IPFS Docs**: https://docs.ipfs.io/

---

## 🎯 **Best Practices**

### **For Property Owners**
- **Regular Verification**: Check ownership status periodically
- **Secure Storage**: Keep property IDs in safe location
- **Documentation**: Maintain offline copies of important data
- **Updates**: Keep wallet and browser updated

### **For Administrators**
- **Access Control**: Limit admin access to trusted personnel
- **Regular Audits**: Review system activity regularly
- **Backup Procedures**: Maintain system backups
- **Emergency Plans**: Have contingency procedures ready

### **For Developers**
- **Testing**: Always test on testnet first
- **Code Review**: Review smart contract code
- **Security**: Follow security best practices
- **Documentation**: Keep documentation updated

---

## 📊 **System Status**

### **Current Status**
- **Network**: Polygon Amoy & Sepolia Testnets
- **Contract**: Deployed and operational
- **Frontend**: Available at localhost:8000
- **Support**: Active development and maintenance

### **Planned Updates**
- **Mainnet Deployment**: Production network launch
- **Mobile App**: Native mobile application
- **API Integration**: Third-party service connections
- **Advanced Features**: Additional functionality

---

*Last Updated: $(Get-Date)*
*Version: 1.0.0*
*Guide Version: 1.0.0*
