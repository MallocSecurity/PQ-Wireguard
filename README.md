# PQ-WireGuard: Post-Quantum Extension Study by Malloc

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Research](https://img.shields.io/badge/Research-Post--Quantum-brightgreen)](https://eprint.iacr.org/2020/379.pdf)

This repository contains Malloc Security's research implementation of a post-quantum secure extension to the WireGuard VPN protocol. Our work builds upon the foundational [PQ-WireGuard paper](https://eprint.iacr.org/2020/379.pdf) by Andreas Hülsing, Kai-Chun Ning, Florian Weber, and Phil Zimmermann, focusing on practical deployment considerations for quantum-resistant cryptography.

## About Malloc's Extension Work

We at **Malloc** are investigating:
- **Practical Deployment**: Real-world implementation challenges
- **Performance Optimization**: Throughput and latency improvements
- **Cryptographic Alternatives**: Evaluation of different PQC algorithms
- **Mobile Integration**: Android/iOS compatibility research
- **Hybrid Approaches**: Combining classical and post-quantum cryptography

## Key Components

| Component | Description |
|-----------|-------------|
| `WireGuard/` | Modified Linux kernel module with PQ enhancements |
| `tamarin/` | Extended formal verification proofs (Tamarin models) |
| `analysis/` | Enhanced security parameter analysis toolkit |
| `benchmarks/` | Comprehensive performance evaluation suite |
| `docs/` | Technical documentation and research findings |

## Getting Started

### Prerequisites
- Linux kernel 5.4+ (headers matching your running kernel)
- GNU Make 4.0+
- GCC 9.0+ or Clang 12.0+
- Python 3.8+ (for benchmarks)
- Tamarin prover 1.6.1+ (for verification)


### 1. Build the Kernel Module
```bash
git clone https://github.com/MallocSecurity/PQ-Wireguard.git
cd pq-wireguard/WireGuard
make
sudo make install
```
  
### 2. Run Benchmarks
```bash 
cd benchmarks
pip install -r requirements.txt
python run_benchmarks.py --scenario=cloud
```

### 3. Verify Security Properties

```bash 
cd tamarin
tamarin-prover --prove pq_wireguard_malloc.spthy
```

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

