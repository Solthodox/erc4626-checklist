# ERC4626 Security checklist

Comprehensive Security Checklist for **ERC4626** Standardized Yield Farming Vaults: A meticulously curated guide outlining essential **security measures** for developers engaging with the ERC4626 standard. This checklist offers a thorough examination of best practices and precautions to fortify the security posture of yield farming vaults, ensuring robust protection against potential threats and vulnerabilities

_Created by [@solthodox](https://twitter.com/solthodox) and [@MaslarovK](https://twitter.com/MaslarovK)_

<p align="center">
  <img src="./img/harvesting.png" width="550"/>
</p>

---

### General Review Approach:
- Read the project's docs, specs, and whitepaper to understand what the smart contracts are meant to do.
- Construct a mental model of what you expect the contracts to look like before checking out the code.
- Do a generic line-by-line review of the contracts.
- Do another review from the perspective of every actor in the threat model.
- Glance over the project's tests + code coverage and look deeper at areas lacking coverage.
- Look at related projects and their audits to check for any similar issues or oversights.

## Standard compliance
- `S1`- Is the vault `EIP20` compatible?
- `S2`- Does `asset`return the address of the token that is deposited in the vault?
- `S3`- Is `asset` a `ERC20` compatible?
---
- `S4`- Does `asset` **never** revert?
- `S5`- Does `totalAssets` **always** include the compounding that occurs from yield?
- `S6`- Does `totalAssets` **always** include any fees externally charged to the vault?
- `S7`- Does `totalAssets` **never** revert?
---
- `S8`- Do  `convertToAssets` & `convertToShares` **always** include any fees externally charged to the vault?
- `S9`- Do `convertToAssets` & `convertToShares` **never** vary depending on the caller?
- `S10`- Do `convertToAssets` & `convertToShares` **never** exclude slippage,vault fees and other chain conditions?
- `S11`- Do `convertToAssets` & `convertToShares` **never** revert, except for a overflow due to a big integer input or similar?
- `S12`- Do `convertToAssets` & `convertToShares` **always** **round down**?
---
- `S13`- Does `maxDeposit` **always** return maximum amount of assets `deposit` would allow to be deposited for `receiver`?
- `S14`- Does `maxMint` **always** return maximum amount of shares `mint` would allow to be minted for `receiver`?
- `S15`- Do `maxDeposit` & `maxMint` **always** return `0` if deposits are disabled?
- `S16`- Do `maxDeposit` & `maxMint` **always** return `2 ** 256 - 1` when there is no limit?
- `S17`- Do `maxDeposit` & `maxMint` **never** relay on `balanceOf` of `asset`?
- `S18`- Do `maxDeposit` & `maxMint` **never** revert?
---
- `S19`- Does `previewDeposit` **always** ignore the `maxDeposit`?
- `S20`- Does `previewMint` **always** ignore the `maxMint`?
- `S21`- Does `previewDeposit` **always** return **same or fewer** shares than those that would actually be minted on `deposit`?
- `S22`- Does `previewMint` **always** return **same or more** assets than those that would actually be deposited on `mint`?
- `S23`- Do `previewDeposit` & `previewMint` **always** include any vault deposit fees ? 
- `S24`- Do `previewDeposit` & `previewMint` **never** revert, except for a overflow due to a big integer input or similar? 
- `S25`- Do `previewDeposit` & `previewMint` **always** consider slippage and other chain conditions unlike `convertToShares` & `convertToAssets`?
- `S26`- Does `previewDeposit` **always** **round down** ?
- `S27`- Does `previewMint` **always** **round up** ?
---
- `S28`- Does `deposit` **always** pull **exactly** `assets` tokens? 
- `S30`- Does `mint` **always** mint **exactly** `shares` shares? 
- `S29`- Does `deposit` **always** mint **exactly** `shares` tokens? 
- `S31`- Does `mint` **always** return the exact amount of `assets` deposited? 
- `S32`- Does `deposit` **always** revert if the `maxDeposit` is exceeded?
- `S33`- Does `mint` **always** revert if the `maxMint` is exceeded?
- `S34`- Do `deposit` & `mint` **always** support the `approve`/`transferFrom` on `asset` as a deposit flow?
- `S35`- Do `deposit` & `mint` **always** emit the `Deposit`event?
---
- `S36`- Does `maxWithdraw` **always** return the maximum amount of assets that a user can `withdraw` ? 
- `S37`- Does `maxRedeem` **always** return the maximum amount of shares that a user can `redeem` ? 
- `S38`- Do `maxWithdraw` & `maxRedeem` **always** return `0` if withdrawals are disabled?
- `S39`- Do `maxWithdraw` & `maxRedeem` **never** revert?
---
- `S40`- Does `previewWithdraw` **always round up**? 
- `S41`- Does `previewRedeem` **always round down**? 
- `S42`- Does `previewWithdraw` **always** return **same or more** `shares` than those that would actually be burnt when performing a `withdraw`? 
- `S43`- Does `previewWithdraw` **always** return **same or fewer** `assets` than those that would actually be sent to the user when performing a `withdraw`? 
- `S44`- Does `previewWithdraw` **always** ignore the `maxWithdraw`?
- `S45`- Does `previewRedeen` **always** ignore the `maxRedeem`?
- `S46`- Do `previewWithdraw` & `previewRedeen` **always** include any vault withdrawal fee?
- `S46`- Do `previewWithdraw` & `previewRedeen` **always** consider slippage and other chain conditions unlike `convertToShares` & `convertToAssets`?
- `S46`- Do `previewWithdraw` & `previewRedeen` **never** revert except for other conditions that would make the `withdraw` & `redeem` revert? 
---
- `S48`- Does `withdraw` **always** transfer **exactly** `assets` assets? 
- `S47`- Does `redeem` **always** burn **exactly** `shares` shares? 
- `S48`- Does `withdraw` **always** burn **exactly** `shares` shares? 
- `S48`- Does `redeem` **always** transfer **exactly** `assets` assets? 
- `S49`- Does `withdraw` **always** revert if `assets` exceeds `maxMint`?
- `S49`- Does `redeem` **always** revert if `shares` exceeds `maxRedeem`?
- `S51`- Do `withdraw` and `redeem` **always** allow to operate on behalf of another `operator` as long as there is an approval of his shares to the `msg.sender` that is decreased after the call?
- `S52`- Do `withdraw` and `redeem` **always** emit the `Withdraw` function?


## Math

## External calls

## Share price


## Inheritance
- `I1`- If the contracts inherits from `ERC4626.sol`, does it override all the needed functions from the contract for the vault to work properly?
- `I2`- Is there any storage collision due to the inheritance?


## Tokens


## Harvest logic


## Permissions and access control


## Security patterns



