# PQ-WireGuard: Post-Quantum Extension Study by Malloc

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Research](https://img.shields.io/badge/Research-Post--Quantum-brightgreen)](https://eprint.iacr.org/2020/379.pdf)

This repository contains Malloc Security's research implementation of a post-quantum secure extension to the WireGuard VPN protocol, building upon the foundational [PQ-WireGuard paper](https://eprint.iacr.org/2020/379.pdf).

## About Malloc's Extension Work

We investigate:
- **Hybrid Cryptography**: Quantum-safe Kyber-1024 + classical X25519
- **Performance Optimization**: AVX2-accelerated NTT operations
- **Mobile Readiness**: ARM64 optimizations for mobile devices
- **Formal Verification**: Extended Tamarin proof models

## Key Components

| Component | Description |
|-----------|-------------|
| `WireGuard/` | Kernel module with PQ enhancements |
| `tamarin/` | Extended formal verification proofs |
| `analysis/` | Security parameter toolkit |
| `benchmarks/` | Performance evaluation suite |
| `docs/` | Technical documentation |

## Installation

### Prerequisites
- Linux kernel 5.4+
- AVX2-capable CPU (Intel Haswell+/AMD Excavator+)
- GCC 9.0+ or Clang 12.0+
- Python 3.8+ (for benchmarks)
- Tamarin prover 1.6.1+ (for verification)


### 1. Compile and Install
#### 1.1 To compile and install PQ-WireGuard:
 Same as the original WireGuard (https://www.wireguard.com/compilation)
One simply needs to use the source code of PQ-WireGuard instead of cloning from the original
git repo at Step 2.

 At the moment PQ-WireGuard only supports Linux. Windows and MacOS are not supported.

   ```bash
  git clone https://github.com/malloc-security/pq-wireguard.git
  cd pq-wireguard/WireGuard
  make -j$(nproc)
  sudo make install
  sudo modprobe wireguard
  ```

#### 1.2 To create a PQ-WireGuard VPN network: see setup_vpn_config.sh as an example
Note that PQ-WireGuard is optimized with AVX2 instructions. Therefore CPUs that do not provide
this instruction set are not supported, which are:
        Intel CPUs before Q2 2013 (Haswell)
        AMD CPUs before Q2 2015 (Excavator)
    (See https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2 for more info)

Running PQ-WireGuard on unsupported CPUs may cause the kernel to crash, and the following error
    message can be expected: illegal hardware instruction (core dumped)

#### 1.3. To run Tamarin symbolic model of PQ-WireGuard:
1. Install Tamarin: https://tamarin-prover.github.io/manual/book/002_installation.html
2. And then cd to tamarin directory and run: tamarin_prover --prove pq_wireguard.spthy
3. Due to memory requirements, it is advised to prove each group of lemmas (or even each lemma)
    separately with the command: tamarin_prover --prove=$GROUP_PREFIX pq_wireguard.spthy
```bash 
cd tamarin
tamarin-prover --prove pq_wireguard_malloc.spthy
```

#### 1.4. Running Dagger Parameter Selection (Python 2)

```bash
# Install required dependencies
pip install numpy matplotlib

# Some systems may require additional backports:
pip install futures backports.functools_lru_cache

# Run parameter selection tool
cd param-select && python2 select_param.py

  
### Current Enhancements

- **Hybrid Handshake**: Quantum-safe + classical ECDH fallback  
- **Optimized Arithmetic**: AVX2-accelerated lattice operations  
- **Enhanced Analysis**: New failure probability modeling  
- **Network Profiles**: AWS/GCP performance baselines  

### Performance Results

| Scenario           | Original (ms) | Malloc (ms) | Improvement |
|--------------------|--------------:|------------:|------------:|
| Handshake          | 4.3           | 3.1         | 28%         |
| 1G Transfer        | 142           | 118         | 17%         |
| 100 Connections    | 890           | 720         | 19%         |

