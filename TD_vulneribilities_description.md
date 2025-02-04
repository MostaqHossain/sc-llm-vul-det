# Different types of Smart Contracts Vulnerabilities
The following discussion will help to understand different kinds of smart contracts vulnerabilities with code snips.

## Overview
A **Timestamp Dependence** vulnerability occurs when a smart contract relies on `block.timestamp` to make critical decisions such as fund transfers, return values, or contract state transitions. This can be exploited because miners can manipulate timestamps slightly to influence contract behavior.

## Vulnerability Detection Criteria
A function is considered vulnerable if it meets the following conditions:

1. **TDInvocation**: The function references `block.timestamp`.
2. **TDAssign**: The timestamp is assigned to a variable that influences logic.
3. **TDContaminate**: The timestamp affects return values or financial operations.

A function is labeled as vulnerable (`label = 1`) if:

**`TDInvocation ∧ (TDAssign ∨ TDContaminate)`**

---

## Examples of Vulnerable Cases

### 1. Assigning `block.timestamp` to a variable (**TDAssign**)
The function `closeRound` assigns `block.timestamp` to `closingTime`, which is then returned. This makes the function susceptible to timestamp manipulation.

![image](https://github.com/user-attachments/assets/a3197aeb-1981-4648-93d6-c4efb0df4d1b)

**Label: 1 (Vulnerable)**

### 2. Using `block.timestamp` to determine return values (**TDContaminate**)
The function `getState` returns different states based on `block.timestamp`. Since return values impact contract logic, this is considered a timestamp dependency vulnerability.

![image](https://github.com/user-attachments/assets/fc98b8e1-5815-457f-ae4c-c357c484e547)

**Label: 1 (Vulnerable)**

### 3. Using `block.timestamp` in financial operations (**TDContaminate**)
The function `releaseAll` uses `block.timestamp` in a `while` condition that controls token transfers. This allows timestamp manipulation to influence fund releases.

![image](https://github.com/user-attachments/assets/4df50ff8-f25b-4a5a-904e-dc6803166a9e)

**Label: 1 (Vulnerable)**

---

## Examples of Safe Cases (Not Vulnerable)

### 1. Timestamp used in a strict condition (**TDAssign**)
The function `withdrawal` assigns `block.timestamp` to a variable but only checks it inside a `require` statement. Since it does not affect logic beyond validation, it is not vulnerable.

![image](https://github.com/user-attachments/assets/0a5e974f-33fd-40a1-a591-8133f545efe0)

**Label: 0 (Safe)**

### 2. Timestamp only used to trigger an exception
The function `Take` checks `block.timestamp` but only throws an error if conditions are unmet. Since it does not influence return values or financial operations, it is not vulnerable.

![image](https://github.com/user-attachments/assets/6dc44977-668c-46ad-a530-294493682b83)

**Label: 0 (Safe)**

---

## Summary

A function has a **Timestamp Dependence Vulnerability** if:
- `block.timestamp` affects logic beyond validation, such as:
  - Assigning it to a variable that impacts return values.
  - Using it in conditions that trigger fund transfers.

A function is **Safe** if:
- `block.timestamp` is only used in validation (`require`) or to throw exceptions.
"""
