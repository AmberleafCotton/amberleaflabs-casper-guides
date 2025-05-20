# Casper Network Validator Self-Identification Guide

## Overview
This guide explains the process of self-identification for Casper Network validators, including how to set up and maintain your validator's public information through the account-info.casper.json file.

## Prerequisites
- A running Casper Network validator node
- A public website domain
- Access to your server's file system
- Validator's secret key

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

### File Location
The account-info.casper.json file must be publicly accessible at:
```
https://[YOUR-DOMAIN]/.well-known/casper/account-info.casper.json
```

## File Structure
### JSON File Requirements
- Must be valid JSON
- Must contain your validator's public key
- Can be placed in either:
  - `nodes` section
  - `affiliated accounts` section

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
-Placeholder Answer-

### Post-Transaction File Management
Q: After the transaction is completed and the data is fetched by the blockchain, can the files be deleted? Can the same process be repeated for another validator using the same domain?
-Placeholder Answer-

## Additional Resources
- [Casper Network Documentation](https://docs.casper.network/)
- [CSPR.live Explorer](https://cspr.live/)
- [Validator Documentation](https://docs.casper.network/operators/)

## Support
For additional support:
- Join the [Casper Discord](https://discord.gg/casperblockchain)
- Visit the [Casper Forum](https://forums.casperlabs.io/) 