# CLVM infinite recursion

## Issue Summary

Chia uses the coinset model, in which everything is a coin. Each coin has a puzzle (or program) that is executed when the coin is spent. 

The puzzle consists of CLVM (Chialisp Virtual Machine) code (https://chialisp.com/).

A malicious CLVM program can be crafted such that, when run, will use an unreasonable amount of memory before it runs out of “cost”.
We charge a cost for every operation and memory allocation in a CLVM program in order to restrict the amount of work and memory it can use from the computer validating a block or transaction. However, we overlooked the cost of allocating space on the virtual machine’s stack.
## Timeline (all times PST) DATE RANGE. All times approximations

10/14/2022 - A researcher submitted a bug report via bugcrowd (https://bugcrowd.com/chia-network-bb) detailing the CLVM infinite recursion issue.

11/03/202 - Version 1.6.1 of the Chia blockchain was released with fixes for the CLVM infinite recursion issue.

## Resolution and recovery

This was issue was addressed in 1.6.1 by:
Validating the proof-of-space in UnfinishedBlocks before executing the CLVM program
Introducing limits on the stack depth when executing CLVM in mempool-mode

The issue was not exploited on mainnet.
