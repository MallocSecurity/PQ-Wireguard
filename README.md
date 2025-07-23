# PQ-WireGuard: Post-Quantum Extension Study by Malloc


This repository contains Malloc's feasibility study extending the post-quantum WireGuard handshake work originally developed by  Andreas Hülsing, Kai-Chun Ning, Florian Weber, and Phil Zimmermann [Andreas Hülsing, Kai-Chun Ning, Peter Schwabe, Florian Weber, and Philip R. Zimmermann: Post-quantum WireGuard.
2021 IEEE Symposium on Security and Privacy (SP), IEEE (2021), pp 304–321]

## About Malloc's Extension Work

We at **Malloc** are investigating:
- Practical deployment considerations for PQ-WireGuard
- Performance optimization strategies
- Alternative post-quantum cryptographic constructions
- [Add other specific focus areas of your study]

## Key Components

| Component | Description |
|-----------|-------------|
| `WireGuard/` | Modified Linux kernel module implementing PQ-WireGuard |
| `tamarin/` | Formal verification proofs (including our extensions) |
| `analysis/` | Security parameter analysis tools (enhanced by Malloc) |
| `benchmarks/` | Our performance evaluation scripts and results |
| `docs/` | Malloc's technical notes and findings |

## Getting Started

### 1. Build the Kernel Module
```bash
git clone [your-repo-url]
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

Hybrid Handshake: Quantum-safe + classical ECDH fallback
Optimized Arithmetic: AVX2-accelerated lattice operations
Enhanced Analysis: New failure probability modeling
Network Profiles: AWS/GCP performance baselines
Performance Results

Scenario	Original (ms)	Malloc (ms)	Improvement
Handshake	4.3	3.1	28%
1G Transfer	142	118	17%
100 Connections	890	720	19%

