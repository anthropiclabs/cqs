# Claude Quantum Safe (CQS)
![cqs](https://github.com/user-attachments/assets/830d05b4-5e72-4b05-97c8-668a20965298)

> Quantum-safe cryptography provider for OpenSSL (≥ v3)

CQS enables post-quantum cryptography (PQC) within standard OpenSSL distributions through a modular provider implementation — integrating quantum-resistant algorithms into existing systems, supporting hybrid cryptographic schemes (classical + PQC), and serving as a testing ground for next-generation cryptographic standards.

---

## Status

| Feature | Support |
|---|---|
| Quantum-safe KEMs for TLS 1.3 | ✅ |
| Post-quantum signatures via OpenSSL EVP | ✅ |
| Hybrid cryptographic schemes (classical + PQC) | ✅ |
| Key storage via X.509, PKCS#12, OpenSSL encode/decode | ✅ |

**With OpenSSL ≥ 3.2:** full TLS 1.3 PQ signature support and CMS compatibility improvements.

---

## Supported Algorithms

Algorithm availability depends on your `liboqs` build configuration.

### Key Encapsulation Mechanisms (KEMs)

- BIKE
- FrodoKEM
- ML-KEM (Kyber family)
- Hybrid variants: `p256_mlkem768`, `x25519_mlkem768`, and more

### Signature Algorithms

- ML-DSA (Dilithium family)
- Falcon
- MAYO
- CROSS
- UOV
- SNOVA
- SLH-DSA (SPHINCS+)

> **Note:** Not all algorithms are enabled for TLS by default. To list available algorithms:
>
> ```sh
> openssl list -kem-algorithms -provider cqs
> openssl list -signature-algorithms -provider cqs
> ```

---

## Hybrid Cryptography

CQS supports hybrid schemes that combine classical cryptography with PQC, enabling gradual migration to quantum-safe systems without breaking existing compatibility.

Examples: `p256_mlkem768`, `x25519_mlkem768`

---

## OpenSSL Compatibility

| OpenSSL Version | Support |
|---|---|
| 3.0 / 3.1 | Limited — no CMS, limited TLS PQ signatures |
| 3.2+ | Full TLS PQ support |
| 3.4+ | KEM testing via `pkeyutl -encap` |
| 3.5+ | Native PQ support — CQS partially bypassed |

### OpenSSL ≥ 3.5.0

Native OpenSSL now includes ML-KEM, ML-DSA, and SLH-DSA. In these environments, CQS automatically disables duplicate implementations and defers to native OpenSSL for stability and performance.

---

## Build & Installation

### Requirements

- OpenSSL ≥ 3.0
- liboqs (latest recommended)
- CMake

### Quick Start

```sh
cmake -S . -B _build
cmake --build _build
ctest --test-dir _build
cmake --install _build
```

### Enable the Provider

After installation, verify the provider is registered:

```sh
openssl list -providers
```

See [`USAGE.md`](USAGE.md) for TLS configuration, key generation, and algorithm testing guides.

---

## Limitations

- Maximum ~44 TLS groups in older OpenSSL versions
- Some servers limit to ~64 signature algorithms
- Algorithm availability depends on `liboqs` build configuration

---

## Architecture

```
┌─────────────────────────────────┐
│        CQS Provider             │
│  (OpenSSL Provider Interface)   │
└────────────────┬────────────────┘
                 │
                 ▼
┌─────────────────────────────────┐
│            liboqs               │
│   (PQ algorithm implementations)│
└─────────────────────────────────┘
```

This modular design enables easy algorithm upgrades, flexible selection, and hybrid cryptography support.

---

## ⚠️ Security Disclaimer

**CQS is experimental software intended for research, testing, and prototyping only.**

- ❌ Not recommended for production use
- ❌ Do not use to protect sensitive or production data

```
THIS SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.
```

---

## Contributing

Contributions are welcome across:

- New PQ algorithm integrations
- Performance improvements
- OpenSSL integration enhancements
- Cross-platform support

See [`CONTRIBUTING.md`](CONTRIBUTING.md) and [`GOVERNANCE.md`](GOVERNANCE.md) for details.

---

## Acknowledgements

Claude Quantum Safe builds on the broader [Open Quantum Safe](https://openquantumsafe.org/) ecosystem, combining academic research, industry collaboration, and open-source contributions.

---

*CQS is part of the transition toward a post-quantum secure internet — enabling developers to experiment today with the cryptography of tomorrow.*
