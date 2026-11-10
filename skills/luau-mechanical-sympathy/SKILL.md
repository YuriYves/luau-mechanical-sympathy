---
name: luau-mechanical-sympathy
description: Performance engineering and optimization for Luau code. Use this skill in situations that demand a high level of optimization.
---

## Objective
The objective of this skill is to provide the agent with the ability to critically analyze the code it has generated, as well as tools to assist in its work. This is intended to develop the agent's intuition regarding when code can, or cannot, be simplified or optimized, while also demonstrating the consequences of premature code optimization.

## Profiling
When a program needs to be optimized, it is necessary to identify which parts are "hot" and worth modifying. This is best accomplished through profiling.

## Tools
Tools for analyzing Luau bytecode are provided, generating a report that assigns a score to the current code based on a "confidence" metric.

Due to the tool's automation, it is designed to provide the agent with a "condensed" and straightforward version, eliminating the need for the agent to analyze the bytecode. In certain situations where the tool may not prove helpful, analyzing the bytecode becomes imperative.