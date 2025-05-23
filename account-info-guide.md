# Casper Network Validator Account Self-Identification Guide

## Overview
This guide explains the process of self-identification for Casper Network validators, including how to set up and maintain your validator's public information through the account-info.casper.json file.

## About This Guide
This is a comprehensive guide designed for validators who want to understand every aspect of the self-identification process. It includes detailed explanations, common questions, and troubleshooting steps to help you implement the account-info standard correctly.

For the official, concise implementation guide, please refer to the [original documentation](https://github.com/make-software/casper-account-info-contract/wiki/Self-Identification-Steps-for-the-Casper-Mainnet-Validators) by Muhammet Kara.

## Prerequisites
### Required Resources
- A running Casper Network validator node
- Validator's secret key
- A public website domain

### Required Software
To run the commands in this guide, you need to have a recent version of `casper-client` and `jq` installed. Run these commands on your node:

```bash
sudo apt update
sudo apt install casper-client jq
```

Verify the installation:
```bash
casper-client --version
jq --version
```

### Payment Requirements
> **Important Payment Information:**
> - Initial `set_url` entry point call requires **15 CSPR** payment
> - Subsequent `set_url` calls require **0.5 CSPR** payment
> - Deploy may fail with "Out of gas" error if insufficient payment amount is provided

## Setup Process

### 1. Directory Structure Setup
✅ **TODO**: Create the following directory structure on your web server:
```
.well-known/
└── casper/
    └── account-info.casper.json
```

### 2. JSON File Creation
✅ **TODO**: Create your account-info.casper.json file

📝 **Reference Examples**:
- [Official Template](https://casper-account-info-example.make.services/.well-known/casper/account-info.casper.json) - Complete reference implementation
- [Casper Delegation](https://casperdelegation.com/.well-known/casper/account-info.casper.json) - Real-world example

⚠️ **Important Validation Steps**:
1. **JSON Schema Validation**:
   - Paste your JSON into the [JSON Schema Validator](https://www.jsonschemavalidator.net/s/ltMuxIEq)
   - You should see the message "No errors found. JSON validates against the schema"
   - This step is crucial for proper display of your information

2. **JSON Syntax Validation**:
   - Validate your JSON syntax at [JSONLint](https://jsonlint.com/)
   - Ensure there are no syntax errors or formatting issues

3. **Content Verification**:
   - Verify all URLs are accessible and working
   - Ensure your validator's public key is correctly listed
   - Check that all required fields are present and properly formatted
   - Verify that all social media links are valid
   - Ensure your domain matches the one you'll use in the set_url transaction

4. **File Location**:
   - The file must be accessible at: `https://[YOUR-DOMAIN]/.well-known/casper/account-info.casper.json`
   - Example: If your website is at `https://mysupercaspervalidator.com`, your file should be at `https://mysupercaspervalidator.com/.well-known/casper/account-info.casper.json`

> **Critical:** If you publish an invalid JSON file, it will not be displayed correctly on block explorers and other dApps. Take the time to validate your file thoroughly.

### 3. File Deployment
✅ **TODO**: Deploy your file to the correct location:
```
https://[YOUR-DOMAIN]/.well-known/casper/account-info.casper.json
```

### 4. Transaction Submission
✅ **TODO**: Submit the set_url transaction:

> **Note:** The URL you provide will be prominently displayed on block explorers as your official website. Your account's public key must exist in the JSON data (either in the nodes or affiliated accounts section) for successful verification by CSPR.live and other dApps in the Casper ecosystem.

```bash
sudo -u casper casper-client put-deploy \
    --chain-name "casper" \
    --node-address "http://127.0.0.1:7777/" \
    --secret-key "/etc/casper/validator_keys/secret_key.pem" \
    --session-hash "fb8e0215c040691e9bbe945dd22a00989b532b9c2521582538edb95b61156698" \
    --session-entry-point "set_url" \
    --payment-amount 15000000000 \
    --session-arg=url:"string='https://YOUR-DOMAIN'"
```

After submitting the transaction:
1. Note the deploy hash returned from the command
2. Wait a few minutes
3. Confirm the deploy succeeded by searching for the hash on [CSPR.live](https://cspr.live/)

### Process Overview
✅ **TODO**: For each additional validator:
1. Add a new entry to the `nodes` array in your existing JSON file
2. Submit a new set_url transaction for each validator
3. Verify the information is displayed correctly

### 5. Verification
✅ **TODO**: Verify the account info for your validator:

1. Clone the Casper Account Info Contract repo:
```bash
cd ~
git clone https://github.com/make-software/casper-account-info-contract.git
```

2. Get the account information file content:
```bash
cd ~/casper-account-info-contract/tools
./get-account-info.sh --node-address=127.0.0.1 --contract-hash=fb8e0215c040691e9bbe945dd22a00989b532b9c2521582538edb95b61156698 --public-key=$(sudo -u casper cat /etc/casper/validator_keys/public_key_hex) | jq
```

You should see the content of your account info file as the output of this command. You can now proceed to CSPR.Live to see your details displayed there: https://cspr.live/validator/YOUR-PUBLIC-KEY

## Best Practices
📝 **Important Notes**:
- Keep your JSON file valid and properly formatted
- Ensure your domain is always accessible
- Update information promptly when changes occur
- Maintain proper security measures for your secret keys

## Troubleshooting
🔍 **Common Issues**:
- Verify JSON file accessibility
- Check transaction status on cspr.live
- Ensure proper CSPR amount for transactions
- Validate JSON format before submission

## FAQ

### File Structure and Location
Q: Is the `.well-known/casper/account-info.casper.json` path fixed by the blockchain/transaction and cannot be modified?
A: Yes, the path `.well-known/casper/account-info.casper.json` is fixed and cannot be modified. You must use this exact path structure for the blockchain to properly verify your validator information.

### Post-Transaction File Management
Q: After the transaction is completed and the data is fetched by the blockchain, can the files be deleted?
A: No, the files must remain accessible at all times. The middleware periodically checks these files to verify the information.

### Multiple Validators Configuration
Q: How do I configure multiple validators using the same domain?
A: You should extend the `nodes` array in your existing `account-info.casper.json` file. Each validator needs:
1. Its own entry in the nodes array
2. A separate set-url transaction
3. A unique description within the same JSON file

## Additional Resources
- [Casper Network Documentation](https://docs.casper.network/)
- [CSPR.live Explorer](https://cspr.live/)
- [Validator Documentation](https://docs.casper.network/operators/)

## Support
For additional support:
- Join the [Casper Discord](https://discord.gg/caspernetwork)
- Visit the [Casper Forum](https://forums.casperlabs.io/) 