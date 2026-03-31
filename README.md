# HeMB — Heterogeneous Media Bonding Protocol

**draft-papadopoulos-hemb-00**
IETF Independent Submission Stream · Informational

## Status

- **Prior art research:** Complete (3 independent passes)
- **Reference implementation:** Production-validated (246 decoded, 0 failed)
- **Field hardware validation:** LoRa+TCP confirmed; LoRa+SBD+SMS pending April 2026
- **RFC submission target:** January 2027

## Problem

In disaster response, maritime, polar, and contested environments, the surviving
communication channels — LoRa mesh radio, Iridium satellite SBD, APRS/AX.25,
SMS, and GSM ring signals — share no common protocol stack. They differ in MTU
by five orders of magnitude (1 byte to 100 KB), in latency by three orders of
magnitude (100 ms to 90 s), and in cost by an unbounded factor (EUR 0 to EUR 0,05/msg).

No existing protocol bonds these bearers simultaneously with coded redundancy.
MPTCP, MP-QUIC, and GRE bonding require IP on every subflow. LACP requires
homogeneous Ethernet. DTN provides store-and-forward but not simultaneous
multi-bearer coding.

## Approach

HeMB bonds N physical bearers into one virtual bearer using Random Linear
Network Coding (RLNC) over GF(256). Any K of N coded symbols from any bearer
combination reconstruct the original payload — arrival order and bearer identity
are irrelevant. Cost-weighted allocation ensures free bearers (LoRa, APRS) are
exhausted before paid bearers (Iridium SBD) receive symbols.

## Novel Contribution

HeMB does not claim novelty in RLNC, any-K-of-N recovery, or multipath
transmission. These are well-established (Ho 2006, RFC 5053, US9537759B2).

HeMB's contribution is the **combination**:

1. **Below-IP operation** on physically incompatible bearers with no shared
   protocol stack — no IP, no TCP, no shared framing
2. **RLNC across latency ratios exceeding 100:1** — up to 900:1 (Iridium SBD
   90s vs ZigBee 100ms) in a single coded generation. Prior art
   (US9537759B2) tested max ~100:1.
3. **Cost-weighted symbol allocation** as a primary constraint — free bearers
   exhausted before paid bearers receive any symbols

No prior system addresses this combination.

## Production Evidence

| Metric | Value |
|--------|-------|
| Generations decoded | 246 |
| Generations failed | 0 |
| Decode latency P50 | 0 ms |
| Decode latency P95 | 25 ms |
| Symbols sent (stress test) | 9,675 |
| Cost incurred | EUR 0,000000 |
| Bugs found and fixed | 10 |
| Tests (3 codebases) | 65 passing |

## Repository Structure

```
spec/
  sections/          RFC draft sections (abstract, intro, related work, etc.)
prior-art/           Three prior art research passes + summary
field-data/          Production hardware validation results
impl/                Reference implementation notes
tools/               Build scripts (xml2rfc)
```

## Reference Implementation

The reference implementation lives in MeshSat:
https://github.com/cubeos-app/meshsat/tree/main/internal/hemb

HeMB is also ported to:
- MeshSat Hub: https://gitlab.nuclearlighters.net/products/meshsat-hub
- MeshSat Android: https://gitlab.nuclearlighters.net/products/meshsat-android

## Related

- [IPoUGRS](https://github.com/papadopouloskyriakos/ipougrs) —
  IP over Unanswered GSM Ring Signals (HeMB bearer type)
- [MeshSat](https://github.com/cubeos-app/meshsat) —
  Multi-transport mesh and satellite gateway (reference implementation)

## Author

Kyriakos Papadopoulos · Leiden, Netherlands
kyriakos@papadopoulos.tech

## License

Apache 2.0 — specification and documentation
Reference implementation: GPLv3 (via MeshSat)
