# 🎯 **Project Usage Guide - Step by Step**

## **🚀 Complete Workflow:**

### **Step 1: Admin Setup (Contract Owner Only)**

1. **Go to:** `http://127.0.0.1:5500/admin.html`
2. **Connect MetaMask** (must be the account that deployed contract)
3. **Add Exporter:**
   - **Exporter Address:** `0x1234567890123456789012345678901234567890` (valid Ethereum address)
   - **University/Info:** `Delhi University` (any info)
   - **Click:** "Add Exporter"
   - **Confirm transaction** in MetaMask

### **Step 2: Document Upload (Exporter Only)**

1. **Switch MetaMask** to exporter account
2. **Go to:** `http://127.0.0.1:5500/upload.html`
3. **Connect MetaMask** (exporter account)
4. **Upload Document:**
   - **Select file** (PDF, image, etc.)
   - **Click:** "Upload to Blockchain"
   - **Confirm transaction** in MetaMask
5. **Save the QR code** for verification

### **Step 3: Document Verification (Anyone)**

1. **Go to:** `http://127.0.0.1:5500/verify.html`
2. **Upload same document** to verify
3. **Click:** "Verify Document"
4. **Result:** ✅ Authentic or ❌ Not Found

---

## **🔧 Address Format Requirements:**

### **✅ Valid Ethereum Address Examples:**

```
0x742d35Cc6635C0532925a3b8D4f25fE4e6548c6B
0xdD2FD4581271e230360230F9337D5c0430Bf44C0
0x1234567890123456789012345678901234567890
```

### **❌ Invalid Address Examples:**

```
123456789                    (too short)
0x12345                      (too short)
0xGGGGGGGGGGG                (invalid characters)
my-wallet-address            (not hex format)
```

---

## **📱 How to Get Valid Addresses:**

### **Method 1: Second MetaMask Account**

1. **MetaMask:** Click profile icon → "Create Account"
2. **Copy address:** Click account name → Copy
3. **Use this address** as exporter

### **Method 2: Friend's Wallet**

1. **Ask friend** to share their MetaMask address
2. **Copy their address:** 0x...
3. **Add as exporter**

### **Method 3: Test Accounts**

Use these test addresses (these are public, don't send real money):

```
0x8ba1f109551bD432803012645Hac136c5185fa1A
0x742d35Cc6635C0532925a3b8D4f25fE4e6548c6B
0xdD2FD4581271e230360230F9337D5c0430Bf44C0
```

---

## **🐛 Common Issues & Solutions:**

### **❌ "Invalid Ethereum address format!"**

**Solution:** Use proper 42-character hex address starting with `0x`

### **❌ "Cannot add your own address as exporter!"**

**Solution:** Use different address than the one currently connected

### **❌ "You need to provide both address & information"**

**Solution:** Fill both fields - address AND university/info

### **❌ "Caller is not the owner"**

**Solution:** Only contract deployer can add exporters

### **❌ "Transaction failed"**

**Solutions:**

- Check you have enough ETH for gas
- Make sure you're on Sepolia network
- Try increasing gas limit in MetaMask

---

## **🎬 Complete Test Scenario:**

### **Account A (Owner - You):**

1. Deploy contract ✅
2. Add Account B as exporter ✅

### **Account B (Exporter):**

1. Connect MetaMask with Account B
2. Upload document
3. Get verification QR code

### **Account C (Verifier - Anyone):**

1. Go to verify page
2. Upload same document
3. Get ✅ Authentic result

---

## **💡 Pro Tips:**

1. **Always test** with small documents first
2. **Save QR codes** - they contain verification info
3. **Check console** (F12) for detailed error messages
4. **Keep some ETH** in all accounts for gas fees
5. **Document the addresses** you add as exporters

**Your project is now fully functional! 🚀**
