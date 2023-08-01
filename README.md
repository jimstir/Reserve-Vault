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



