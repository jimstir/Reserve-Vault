---
eip: 7425
title: Tokenized Reserve
description: Reserve with user auditing
author: Jimmy Debe (@jimstir)
discussions-to: https://ethereum-magicians.org/t/eip-7425-tokenized-reserve/15297
status: Draft
type: Standards Track
category: ERC
created: 2023-06-30
requires: 20, 4626
---

# Reserve-Vault

## Description
This is a reference implementations of EIP 7425. There are getter functions, and intializled contructor parameters included. Continued development will include added test, scripts, extension contracts and other documentation.

## Abstract

A proposal for a tokenized reserve mechanism. The reserve allows an audit of on-chain actions of the owner. Using [ERC-4626](../EIPS/eip-4626.md), stakeholders can create shares to show support for actions in the reserve.

## Motivation

Tokenized reserves are an extension of tokenized vaults. The goal is to create a reserve similar to a real world reserve an entity has as a backup incase regular funds run low. In the real world, an entity will have certain criteria to access reserve funds. In a decentralized environment, an entity can incorporate stakeholders into their criteria. Hopefully this will help entities who participate in decentralized environments to be transparent with stakeholders.

## Specification

### Definitions:

  - owner: The creator of the reserve.
  - user: Stakeholders of specific proposals
  - reserve: The tokenized reserve contract
  - proposal: Occurs when the owner wants a withdrawal from contract
 
### Constructor:
 
  - name: ERC-20 token name
  - ticker: ERC-20 ticker
  - asset: ERC-4626 underlying ERC-20 address
  - rAuth: Primary authorized user
  - rOwner: Owner of the Reserve

# Getter Functions:

#### whosOwner
> - Get the reserve owner
  ```yaml
  - name: whosOwner
	  type: function
	  stateMutability: view

	inputs: [ ]

	outputs:
		- name: _rOwner
		  type : address
   
  ```

#### whosAuth
> - Get the reserve primary authorized user
  ```yaml
  - name: whosAuth
	  type: function
	  stateMutability: view

	inputs: [ ]

	outputs:
		- name: _rAuth
		  type : address
   
  ```

#### proposalCheck
> - Check current total of opened proposals
  ```yaml
  - name: proposalCheck
	  type: function
	  stateMutability: view

	inputs: [ ]

	outputs:
		- name: _proposalNum
		  type : uint256

  ```

#### getAuth
> - Authorized users of the reserve
  ```yaml
  - name: getAuth
	  type: function
	  stateMutability: view

	inputs: [ ]

	outputs:
		- name: _authUsers
		  type : bool

  ```

#### accountCheck
> - Get number of deposits made to reserve by the owner
> - MUST BE deposits made by calling depositReserve function
  ```yaml
  - name: accountCheck
	  type: function
	  stateMutability: view

	inputs: [ ]

	outputs:
		- name: _ownerDeposits
		  type : uint256

  ```

#### depositTime
> - Get time of a deposit made to reserve by the owner
  ```yaml
  - name: depositTime
	  type: function
	  stateMutability: view

	inputs:
	- name: count
	type: uint256

	outputs:
		- name: ownerBook.time
		type : uint256

  ```

#### ownerDeposit
> - Get time of a deposit made to reserve by the owner
  ```yaml
  - name: ownerDeposit
	  type: function
	  stateMutability: view

	inputs:
	- name: count
	type: uint256

	outputs:
		- name: ownerBook.deposit
		type : uint256

  ```

#### tokenDeposit
> - Token type deposited to contract by the owner
  ```yaml
  - name: tokenDeposit
	  type: function
	  stateMutability: view

	inputs:
	- name: count
	type: uint256

	outputs:
		- name: ownerBook.token
		type : address

  ```

#### userDeposit
> - Token type deposited to contract by the owner
> - MUST be an ERC20 address
  ```yaml
  - name: userDeposit
	  type: function
	  stateMutability: view

	inputs:
	- name: user
	type: address
	- name: proposal
	type: uint256

	outputs:
		- name: userBook.deposit
		type : uint256

  ```

#### userWithdrew
> - Amount withdrawn from given proposal by the user
> - MUST be an ERC20 address
  ```yaml
  - name: userWithdrew
	  type: function
	  stateMutability: view

	inputs:
	- name: user
	type: address
	- name: proposal
	type: uint256

	outputs:
		- name: userBook.withdrew
		type : uint256

  ```

#### userNumOfProposal
> - The total number of proposals joined by the user
  ```yaml
  - name: userNumOfProposal
	  type: function
	  stateMutability: view

	inputs:
	- name: user
	type: address

	outputs:
		- name: userBook.numOfProposals
		type : uint256

  ```

#### userProposal
> - The proposal number from the specific proposal joined by the user
> - MUST NOT be zero
  ```yaml
  - name: userProposal
	  type: function
	  stateMutability: view

	inputs:
	- name: user
	type: address
	- name: proposal
	type: uint256

	outputs:
		- name: userBook.proposal
		type : uint256

  ```

#### proposalToken
> - Token used for given proposal
> - MUST be ERC20 address
  ```yaml
  - name: proposalToken
	  type: function
	  stateMutability: view

	inputs:
	- name: proposal
	type: uint256

	outputs:
		- name: proposalBook.token
		type : uint256

  ```

#### proposalWithdrew
> - Amount withdrawn for given proposal
  ```yaml
  - name: proposalWithdrew
	  type: function
	  stateMutability: view

	inputs:
	- name: proposal
	type: uint256

	outputs:
		- name: proposalBook.withdrew
		type : uint256

  ```

#### proposalDeposit
> - Amount received for given proposal
  ```yaml
  - name: proposalDeposit
	  type: function
	  stateMutability: view

	inputs:
	- name: proposal
	type: uint256

	outputs:
		- name: proposalBook.received
		type : uint256

  ```

#### totalShares
> - Total shares issued for a given proposal
  ```yaml
  - name: totalShares
	  type: function
	  stateMutability: view

	inputs:
	- name: proposal
	type: uint256

	outputs:
		- name: _totalShares
		type : uint256

  ```

#### closedProposal
> - Check if proposal is closed
  ```yaml
  - name: closedProposal
	  type: function
	  stateMutability: view

	inputs:
	- name: proposal
	type: uint256

	outputs:
		- name: _closedProposal
		type : uint256

  ```

#### addAuth
> - Add a new authorized user
  ```yaml
  - name: addAuth
	  type: function
	  stateMutability: nonpayable

	inputs:
	- name: author
	type: address

	outputs:
		- []

  ```

#### proposalDeposit
> - Make a deposit to proposal creating new shares
> - MUST be open proposal
> - MUST NOT be closed proposal
  ```yaml
  - name: proposalDeposit
	  type: function
	  stateMutability: nonpayable

	inputs:
	- name: assets
	type: uint256
	-name: receiver
	type: address
	-name: proposal
	type: uint256

	outputs:
		- name: shares
		type: uint256

  ```

#### proposalMint
> - Make a deposit to proposal creating new shares
> - MUST be open proposal
> - MUST NOT be closed proposal
  ```yaml
  - name: proposalMint
	  type: function
	  stateMutability: nonpayable

	inputs:
	- name: shares
	type: uint256
	-name: receiver
	type: address
	-name: proposal
	type: uint256

	outputs:
		- name: assets
		type: uint256

  ```

#### proposalWithdrawal
> - Burn shares, receive 1 to 1 value of assets
> - MUST have closed proposalNumber
> - MUST have userDeposit greater than or equal to userWithdrawal
  ```yaml
  - name: proposalWithdrawal
	  type: function
	  stateMutability: nonpayable

	inputs:
	- name: assets
	type: uint256
	-name: receiver
	type: address
	-name: owner
	type: address
	-name: proposal
	type: uint256

	outputs:
		- name: shares
		type: uint256

  ```

#### proposalRedeem
> - Burn shares, receive 1 to 1 value of assets
> - MUST have open proposal number
> - MUST have userDeposit greater than or equal to userWithdrawal
  ```yaml
  - name: proposalRedeem
	  type: function
	  stateMutability: nonpayable

	inputs:
	- name: shares
	type: uint256
	-name: receiver
	type: address
	-name: owner
	type: address
	-name: proposal
	type: uint256

	outputs:
		- name: assets
		type: uint256

  ```

#### proposalOpen
> - Issue new proposal
> - MUST create new proposal number
> - MUST account for amount withdrawn
  ```yaml
  - name: proposalOpen
	  type: function
	  stateMutability: nonpayable

	inputs:
	- name: token
	type: address
	-name: amount
	type: uint256
	-name: receiver
	type: address
	-name: num
	type: uint256

	outputs:
		- name: proposalNum
		type: uint256

  ```

#### proposalClose
> - Close an opened proposal
> - MUST account for amount received
> - MUST have proposal greater than current proposal
  ```yaml
  - name: proposalClose
	  type: function
	  stateMutability: nonpayable

	inputs:
	- name: token
	type: address
	-name: proposal
	type: uint256
	-name: amount
	type: uint256
	-name: num
	type: uint256

	outputs:
		- name: close
		type: bool

  ```

#### depositSnail
> - Optional accounting for tokens deposited by owner
> - MUST be reserve rOwner
  ```yaml
  - name: depositSnail
	  type: function
	  stateMutability: nonpayable

	inputs:
	- name: token
	type: address
	-name: sender
	type: address
	-name: amount
	type: uint256

	outputs:
		- name: deposit
		type: bool

  ```

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 and RFC 8174.

## Rationale

The reserve is designed to be a basic implementation of the reserve interface. Other non specified conditions should be addressed on case by case instances. Reserves use [ERC-20](../EIPS/eip-20.md) for shares and [ERC-4626](../EIPS/eip-4626.md) for creations of shares within proposals. All accounting measures are designed to enforce audit practices based on use case. 

## Backwards Compatibility

Tokenized reserves are made compatible with [ERC-20](../EIPS/eip-20.md) and [ERC-4626](../EIPS/eip-4626.md).

## Security Considerations

Needs discussion.

## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).

