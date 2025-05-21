# Casper Network Validator Account Self-Identification Guide

## Overview
This guide explains the process of self-identification for Casper Network validators, including how to set up and maintain your validator's public information through the account-info.casper.json file.

## About This Guide
This is a comprehensive guide designed for validators who want to understand every aspect of the self-identification process. It includes detailed explanations, common questions, and troubleshooting steps to help you implement the account-info standard correctly.

For the official, concise implementation guide, please refer to the [original documentation](https://github.com/make-software/casper-account-info-contract/wiki/Self-Identification-Steps-for-the-Casper-Mainnet-Validators) by Muhammet Kara.

## Prerequisites
Before starting the self-identification process, ensure you have:

1. **Validator Node**
   - A running Casper Network validator node
   - Validator's secret key

2. **Website Requirements**
   - A public website domain
   - Ability to create and modify files on your web server
   - Proper file permissions for the `.well-known` directory

3. **Required Software**
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

## Process Overview
1. [Initial Setup](#initial-setup)
2. [File Structure](#file-structure)
3. [Transaction Process](#transaction-process)
4. [Multiple Validators](#multiple-validators)

## Initial Setup
### Required Directory Structure
Create the following directory structure on your web server:
```
.well-known/
└── casper/
    └── account-info.casper.json
```

### Live Examples
Since the account-info.casper.json file must be publicly accessible, you can view examples from active validators on the network:
- [Official Template](https://casper-account-info-example.make.services/.well-known/casper/account-info.casper.json) - Reference implementation with all possible fields

- [Casper Delegation](https://casperdelegation.com/.well-known/casper/account-info.casper.json) - Example of a validator using the account info schema

### File Location
The account-info.casper.json file must be publicly accessible at:
```
https://[YOUR-DOMAIN]/.well-known/casper/account-info.casper.json
```

## File Structure
### JSON File Requirements
- Must be valid JSON
- Must contain your validator's public key
- Must be publicly accessible at the specified path

### JSON Template and Validation
1. Download the template:
   - Use the [official template](https://casper-account-info-example.make.services/.well-known/casper/account-info.casper.json) as your starting point
   - Right-click and select "Save Link As..." to download the sample file

2. Validate your JSON:
   - Use [JSON Schema Validator](https://www.jsonschemavalidator.net/s/ltMuxIEq) to validate against the standard schema
   - Use [JSONLint](https://jsonlint.com/) to check JSON validity
   - You should see "No errors found. JSON validates against the schema" message

3. Important Validation Notes:
   - Invalid JSON files will not be displayed correctly
   - Always validate before publishing
   - Check all required fields are present
   - Ensure all URLs are accessible

4. Example Structure:
   - See the [official template](https://casper-account-info-example.make.services/.well-known/casper/account-info.casper.json) for the complete JSON structure
   - Replace all placeholder values with your validator's information
   - Keep the same structure and formatting

## Transaction Process
### Setting the URL
1. Initial deployment requires 15 CSPR
2. Subsequent updates require 0.5 CSPR
3. Use the following command structure:
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

## Multiple Validators
### Process for Each Validator
1. Create a new JSON file with:
   - Different validator key
   - Unique description
2. Submit a new set_url transaction
3. Repeat verification process

### Important Notes
- Each validator requires a separate transaction
- Each validator can have its own unique description
- The JSON file can be removed after verification

## Best Practices
1. Keep your JSON file valid and properly formatted
2. Ensure your domain is always accessible
3. Update information promptly when changes occur
4. Maintain proper security measures for your secret keys

## Troubleshooting
- Verify JSON file accessibility
- Check transaction status on cspr.live
- Ensure proper CSPR amount for transactions
- Validate JSON format before submission

## FAQ

### File Structure and Location
Q: Is the `.well-known/casper/account-info.casper.json` path fixed by the blockchain/transaction and cannot be modified? For example, can I use different paths like:
```
.well-known/validator1/account-info.casper.json
.well-known/validator2/account-info.casper.json
```
A: Yes, the path `.well-known/casper/account-info.casper.json` is fixed and cannot be modified. You must use this exact path structure for the blockchain to properly verify your validator information.

### Post-Transaction File Management
Q: After the transaction is completed and the data is fetched by the blockchain, can the files be deleted? Can the same process be repeated for another validator using the same domain?
A: No, the files must remain accessible at all times. The middleware periodically checks these files to verify the information. However, you don't need separate files for multiple validators - you can have multiple nodes listed in the same JSON file.

### Multiple Validators Configuration
Q: How do I configure multiple validators using the same domain?
A: You should extend the `nodes` array in your existing `account-info.casper.json` file. Each validator needs:
1. Its own entry in the nodes array
2. A separate set-url transaction
3. A unique description within the same JSON file

### Security and Verification
Q: What prevents someone from listing other people's nodes as their own in the JSON file?
A: Each node must submit its own set-url transaction signed with its private key. Without this transaction, the node's information won't be verified or displayed, even if it's listed in the JSON file.

### Transaction Requirements
Q: Do I need to submit a transaction for each node listed in the JSON file?
A: Yes, each node must submit its own set-url transaction signed with its private key. This is required for:
- Nodes listed in the `nodes` array
- Accounts listed in the `affiliated accounts` section
Without the corresponding transaction, the information won't be verified or displayed.

### Command Updates
Q: Is there an updated command for the set-url transaction?
A: The old command (put-deploy) still works, though it may show a deprecation warning. The updated command (put-transaction) is not yet a high priority as the old command remains functional.

### JSON File Structure
Q: How should I structure the JSON file for multiple validators?
A: You should extend the `nodes` array in your existing file. Do not create separate files or copy-paste the entire JSON structure. Each node should have its own entry in the array with its unique description and information.

### Verification Process
Q: How is the information verified?
A: The process involves multiple steps:
1. The JSON file must be publicly accessible at the correct path
2. Each node must submit a set-url transaction
3. The middleware periodically checks the file
4. Only information from nodes that have submitted transactions is displayed

### Example Implementations
For reference implementations, check:
- https://casper-account-info-example.make.services/.well-known/casper/account-info.casper.json
- https://casperdelegation.com/.well-known/casper/account-info.casper.json

## Additional Resources
- [Casper Network Documentation](https://docs.casper.network/)
- [CSPR.live Explorer](https://cspr.live/)
- [Validator Documentation](https://docs.casper.network/operators/)

## Support
For additional support:
- Join the [Casper Discord](https://discord.gg/caspernetwork)
- Visit the [Casper Forum](https://forums.casperlabs.io/) 