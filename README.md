> **Note:**
> ✅ Registered subnetwork with netuid: 421 on Bittensor testnet.

<div align="center">

# **TensorUSD: TAO-Backed StableCoin Subnet** <!-- omit in toc -->

[![Website](https://img.shields.io/badge/Website-tensorusd.com-blue)](https://tensorusd.com)
[![Discord Chat](https://img.shields.io/discord/308323056592486420.svg)](https://discord.gg/bittensor)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) 

---

## The Incentivized Internet <!-- omit in toc -->

[Discord](https://discord.gg/bittensor) • [Network](https://taostats.io/) • [Research](https://bittensor.com/whitepaper)
</div>

---
- [Quickstarter template](#quickstarter-template)
- [Introduction](#introduction)
  - [Example](#example)
- [Installation](#installation)
  - [Before you proceed](#before-you-proceed)
  - [Install](#install)
- [Writing your own incentive mechanism](#writing-your-own-incentive-mechanism)
- [Writing your own subnet API](#writing-your-own-subnet-api)
- [Subnet Links](#subnet-links)
- [License](#license)

---


## Quickstarter: TensorUSD (tensorusd.com)

TensorUSD is a TAO-backed stablecoin subnet, providing a decentralized, incentive-driven mechanism for stablecoin issuance and management. The subnet leverages Bittensor's protocol to coordinate validators and miners for stablecoin operations, including minting, redemption, and stability enforcement.


Key features:
- TAO-backed stablecoin issuance and redemption
- Decentralized stability mechanism via subnet validators
- Extensible protocol for custom incentive logic
- Easy integration with Bittensor's Yuma Consensus
- Official domain: [tensorusd.com](https://tensorusd.com)

This repository abstracts away blockchain complexity, allowing you to focus on stablecoin logic and incentive design. Customize the protocol and neuron scripts to fit your requirements.


## Introduction

**TensorUSD** ([tensorusd.com](https://tensorusd.com))

TensorUSD enables the creation and management of a stablecoin backed by TAO, the native token of Bittensor. Validators and miners collaborate to maintain the stability and integrity of the stablecoin, using incentive mechanisms and consensus logic defined in the protocol.


The subnet consists of:
- Stablecoin miners: handle minting, redemption, and reporting operations. **(Testnet: netuid 421)**
- Stablecoin validators: enforce stability, validate transactions, and score miner performance. **(Testnet: netuid 421)**
- Custom protocol: defines the rules for stablecoin operations and consensus
- Integration with Bittensor's Yuma Consensus for onchain coordination

To implement your own stablecoin logic, update the protocol and neuron scripts as described below.
## Installation

### Before you proceed
Before you proceed with the installation of the subnet, note the following: 

- Use these instructions to run your subnet locally for your development and testing, or on Bittensor testnet or on Bittensor mainnet. 
- **IMPORTANT**: We **strongly recommend** that you first run your subnet locally and complete your development and testing before running the subnet on Bittensor testnet. Furthermore, make sure that you next run your subnet on Bittensor testnet before running it on the Bittensor mainnet.
- You can run your subnet either as a subnet owner, or as a subnet validator or as a subnet miner. 
- **IMPORTANT:** Make sure you are aware of the minimum compute requirements for your subnet. See the [Minimum compute YAML configuration](./min_compute.yml).
- Note that installation instructions differ based on your situation: For example, installing for local development and testing will require a few additional steps compared to installing for testnet. Similarly, installation instructions differ for a subnet owner vs a validator or a miner. 

### Install

- **Running locally**: Follow the step-by-step instructions described in this section: [Running Subnet Locally](./docs/running_on_staging.md).
- **Running on Bittensor testnet**: Follow the step-by-step instructions described in this section: [Running on the Test Network](./docs/running_on_testnet.md).
- **Running on Bittensor mainnet**: Follow the step-by-step instructions described in this section: [Running on the Main Network](./docs/running_on_mainnet.md).

---


## Customizing StableCoin Logic

To build your own TAO-backed stablecoin mechanism, update the following files:
- `template/protocol.py`: Define the stablecoin protocol (mint, redeem, report, etc.)
- `neurons/miner.py`: Implement miner logic for stablecoin operations
- `neurons/validator.py`: Implement validator logic for stability enforcement and scoring
- `template/forward.py`: Define validator forward pass for transaction validation
- `template/reward.py`: Specify how validators reward miners for correct stablecoin operations

Also update:
- `README.md`: Project documentation
- `CONTRIBUTING.md`: Contribution guidelines
- `template/__init__.py`: Project version
- `setup.py`: Project metadata
- `docs/`: Project documentation

__Note__
Rename the `template` directory to match your stablecoin project name.


# TensorUSD Subnet API
To interact with the TensorUSD subnet, implement a standardized API interface using Bittensor's `SubnetsAPI`. This allows clients to mint, redeem, and query stablecoin operations via exposed axons.

Implement the following abstract methods in your API handler:
- `prepare_synapse`: Prepare stablecoin transaction payloads (mint, redeem, report)
- `process_responses`: Process network responses for stablecoin operations

Example usage:
```python
from bittensor.subnets import SubnetsAPI
from StableCoinSubnet import MintSynapse

class TensorUSDAPI(SubnetsAPI):
  def __init__(self, wallet: "bt.wallet"):
    super().__init__(wallet)
    self.netuid = <your_netuid>

  def prepare_synapse(self, amount: float, recipient: str) -> MintSynapse:
    # Prepare mint transaction
    synapse = MintSynapse(amount=amount, recipient=recipient)
    return synapse

  def process_responses(self, responses):
    # Process mint results
    for response in responses:
      if response.dendrite.status_code == 200:
        return response.result
    return None
```

# Subnet Links
See `subnet_links.py` or access via:
```python
import template
template.SUBNET_LINKS
```
## License
This repository is licensed under the MIT License.
```text
# The MIT License (MIT)
# Copyright © 2024 Opentensor Foundation

# Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated
# documentation files (the “Software”), to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software,
# and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

# The above copyright notice and this permission notice shall be included in all copies or substantial portions of
# the Software.

# THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO
# THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
# OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
# DEALINGS IN THE SOFTWARE.
```
# TensorUSD
