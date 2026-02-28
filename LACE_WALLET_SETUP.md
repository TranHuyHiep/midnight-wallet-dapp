# Lace Wallet Integration - Current Status

## ✅ What Works

1. **Wallet Connection**: Lace wallet connects successfully to the dApp
2. **Wallet Sync**: The wallet syncs with the network (`Wallet.Sync` events)
3. **Balance Queries**: Can read shielded/unshielded balances and dust
4. **Address Display**: Unshielded and shielded addresses are accessible

## ⚠️ Known Issues

### 1. Faucet Feature (SDK Compatibility)

**Issue**: The built-in faucet feature fails with `TypeError: this.shielded.finalizeTransaction is not a function`

**Root Cause**: Version incompatibility between:
- `@midnight-ntwrk/wallet-sdk-facade@2.0.0-rc.1` 
- `@midnight-ntwrk/wallet-sdk-shielded@2.0.0-rc.4`

The `WalletFacade.finalizeRecipe()` method expects the shielded wallet to have a `finalizeTransaction()` method that doesn't exist in this RC version.

**Workaround**: 
- The faucet button now displays current balances instead of attempting a transfer
- **Pre-fund your Lace wallet** with NIGHT tokens using an external faucet or transfer
- Ensure your wallet has dust (generated automatically from NIGHT balance)

**Permanent Fix** (requires upstream changes):
```bash
# Option A: Wait for stable wallet-sdk releases
# Option B: Update package.json when compatible versions are available
"@midnight-ntwrk/wallet-sdk-facade": "^2.0.0",  # stable version
```

### 2. Contract Deployment/Interaction

**Status**: Not yet tested - may have similar SDK compatibility issues

**Next Steps**:
1. Ensure wallet has NIGHT tokens and dust
2. Try deploying a contract via the "Deploy Contract" button
3. If it fails with similar `finalizeTransaction` errors, the same SDK issue affects contract operations

## 🔧 Configuration Changes Made

### Docker Port Mappings (`compose.yml`)

Added host port mappings to allow browser connections:

```yaml
node:
  ports:
    - '9943:9944'  # Original mapping
    - '9944:9944'  # Added for localhost:9944 WebSocket access

indexer:
  ports:
    - '9087:9088'  # Original mapping  
    - '8088:8088'  # Added for localhost:8088 HTTP/WS access
```

### Frontend Changes (`src/App.tsx`)

1. **Wallet Connect Fallback**:
   - Tries `initialAPI.connect(networkId)` first
   - Falls back to `initialAPI.connect()` for CIP-30 style wallets
   - Added debug logging for troubleshooting

2. **Faucet Workaround**:
   - Disabled actual faucet transfer
   - Shows current wallet balances instead
   - Displays warnings if dust is missing

## 📋 Setup Instructions

### Prerequisites

1. **Install Lace Wallet** browser extension
2. **Fund your Lace wallet** with NIGHT tokens:
   - Use the Midnight testnet faucet (if available)
   - Or transfer from another wallet

### Running the dApp

```bash
# Start services
docker compose up -d

# Verify services are running
docker compose ps

# Check logs if needed
docker logs -f midnight-wallet-dapp-dapp-1
```

### Connecting Lace

1. Open http://localhost:8080 in your browser
2. Ensure Lace extension is unlocked
3. Select network: `undeployed` (or match your local network)
4. Click "Connect Wallet"
5. Approve the connection in Lace popup
6. Check console for `[Connect] ConnectedAPI methods:` debug output

### Checking Balances

1. After connecting, click "Request Dust from Faucet"
2. The app will display your current balances:
   - Shielded balance (STAR)
   - Unshielded balance (STAR)  
   - Dust balance and cap

## 🐛 Debugging

### Enable Console Logging

Open DevTools → Console and look for:
- `[Connect]` - Wallet connection logs
- `[Deploy]` - Contract deployment logs
- `transferUnshieldedFromFaucet:` - Faucet operation logs

### Common Errors

**"No Midnight wallet detected"**
- Ensure Lace extension is installed and enabled
- Refresh the page
- Check `window.midnight` in console

**"WebSocket connection failed"**
- Verify Docker ports are mapped correctly
- Check `docker compose ps` shows healthy services
- Try `curl http://localhost:9944/health` (should return HTTP 200)

**"API-WS: disconnected from ws://localhost:9944"**
- This is normal during wallet sync
- Wait for sync to complete
- Check node logs: `docker compose logs node`

## 🚀 Next Steps

### Short Term
1. Pre-fund Lace wallet with NIGHT tokens
2. Test contract deployment with funded wallet
3. Document contract interaction results

### Long Term  
1. Update to stable `wallet-sdk-facade` when available
2. Re-enable faucet feature after SDK fix
3. Add comprehensive error handling for SDK version mismatches

## 📦 Package Versions

Current versions in `package.json`:

```json
{
  "@midnight-ntwrk/dapp-connector-api": "4.0.0",
  "@midnight-ntwrk/wallet-sdk-facade": "2.0.0-rc.1",
  "@midnight-ntwrk/midnight-js-contracts": "3.1.0",
  "@midnight-ntwrk/ledger-v7": "7.0.0"
}
```

Transitive dependency:
- `@midnight-ntwrk/wallet-sdk-shielded@2.0.0-rc.4` (via facade)

## 📞 Support

For SDK compatibility issues, check:
- [Midnight GitHub Issues](https://github.com/midnight-ntwrk)
- Midnight Discord/Community channels
- Wait for stable v2.0.0 wallet SDK releases
