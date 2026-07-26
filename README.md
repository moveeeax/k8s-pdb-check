# k8s-pdb-check

> Find the PodDisruptionBudget that will stall your drain, before you start draining.

**Status:** 🚧 In development

## Overview

Report PodDisruptionBudgets that would block a node drain before you start one, including budgets that can never be satisfied because minAvailable equals the replica count.

## Features

- Simulates a drain of a given node and reports every PDB that would refuse the eviction
- Flags unsatisfiable budgets where `minAvailable` equals the workload's replica count, so no pod can ever be evicted
- Catches `maxUnavailable: 0` and budgets whose selector currently matches no pods at all
- Reads `disruptionsAllowed` and `currentHealthy` from PDB status, so a temporarily unhealthy workload is distinguished from a permanently broken budget
- Node-scoped with `--node`, or cluster-wide as a pre-upgrade sweep
- Non-zero exit when a blocking budget is found, for gating a rolling upgrade pipeline

## Stack

Go + client-go (policy/v1 and the eviction subresource) with cobra.

## Usage

```bash
k8s-pdb-check --node worker-03 --output table --fail-on blocking
```

## License

MIT
