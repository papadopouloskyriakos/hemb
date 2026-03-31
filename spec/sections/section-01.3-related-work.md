## TASK 2 — SECTION 1.3: RELATED WORK

### 1.3.  Related Work

   Multipath transport is a well-studied area.  This section surveys
   the most relevant prior work and identifies the specific gap HeMB
   addresses.

   **Multipath Transport Protocols.**  Multipath TCP (MPTCP) [RFC8684]
   and Multipath QUIC [RFC9000] enable a single transport connection
   to utilize multiple IP-layer paths simultaneously.  Both require
   IP connectivity and a TCP or UDP protocol stack on every subflow.
   Neither incorporates network coding; redundancy is achieved through
   retransmission.  The typical latency ratio between subflows is
   modest (approximately 1:5 for WiFi versus cellular).  These
   protocols are inapplicable when bearers lack IP connectivity.

   **IP-Layer and Link-Layer Bonding.**  GRE Tunnel Bonding [RFC8157]
   aggregates multiple IP tunnels into a single virtual link.  LACP
   [IEEE802.1AX] bonds homogeneous Ethernet links.  TETRA multi-slot
   allocation bonds identical TDMA radio slots.  All assume either IP
   encapsulation on every constituent link or physical homogeneity
   across links.  None support bearers that lack a shared protocol
   stack.

   **Delay-Tolerant Overlays.**  The Bundle Protocol version 7
   [RFC9171] provides store-and-forward message delivery across
   disrupted networks.  Reticulum [MESHSAT2026] provides a
   cryptographic routing layer for delay-tolerant and low-bandwidth
   networks.  Both operate sequentially: a message is forwarded on one
   bearer at a time, with custody transfer ensuring eventual delivery.
   Neither bonds multiple bearers simultaneously with coded redundancy.
   HeMB's N=1 fallback mode incorporates DTN custody concepts —
   acknowledging that sequential single-bearer delivery remains the
   correct strategy when only one bearer is available.

   **Network-Coded Multipath.**  Random Linear Network Coding (RLNC)
   for multipath communication is an established technique.  Ho et al.
   [HO2006] proved the theoretical foundation for RLNC multicast.
   Raptor codes [RFC5053] and RaptorQ [RFC6330] formalized any-K-of-N
   symbol recovery as fountain codes.  US Patent 9,537,759 B2
   [US9537759B2], assigned to MIT and funded by DARPA, applies RLNC
   across heterogeneous TCP/IP subflows with different round-trip
   times.  This is the closest prior art to HeMB.  The key technical
   distinction is that US9537759B2 requires a TCP/IP stack on every
   subflow; the maximum tested round-trip time ratio is approximately
   100:1 (1500 ms versus 15 ms).  Coded-MPTCP (C-MPTCP) applies RLNC
   over MPTCP subflows, inheriting the same IP requirement.  Loss
   Tolerant Bandwidth Aggregation (LTBA) [WU2014] applies block-level
   forward error correction across cellular and WiFi, operating above
   IP with no cost-awareness model and no RLNC.  All systems in this
   cluster require IP connectivity on every constituent path.

   **IoT-Specific Systems.**  GDEP [LYSOGOR2019] bridges LoRa sensor
   networks to Iridium satellite gateways for remote IoT telemetry.
   GDEP operates sequentially — a message is collected via LoRa and
   then forwarded via Iridium as separate, independent transmissions.
   It does not bond the bearers simultaneously and does not apply
   coding.  LoRaHop [LORAHOP2023] applies RLNC flooding within
   single-technology LoRa mesh networks to improve multi-hop
   throughput.  It operates on a single bearer type and does not
   address cross-bearer bonding.  Both systems confirm the relevance
   of the problem space — heterogeneous IoT communication across
   dissimilar radio technologies — but neither solves the simultaneous
   multi-bearer bonding problem.

   **Summary of Contribution.**  HeMB does not claim novelty in RLNC,
   in any-K-of-N symbol recovery, or in multipath transmission as
   individual concepts.  These are well-established [HO2006] [RFC5053]
   [US9537759B2].  HeMB's contribution is the integration of these
   techniques into a sub-IP bearer bonding protocol that operates
   across physically incompatible bearers — bearers that share no
   common protocol stack, differ in MTU by five orders of magnitude
   (1 byte to 100 kilobytes), differ in one-way latency by up to three
   orders of magnitude (100 milliseconds to 90 seconds), and differ in
   per-message cost by an unbounded factor (zero to EUR 0,05).  No
   prior system addresses this combination.  Specifically:

   (1) No prior system bonds heterogeneous non-IP bearers
       simultaneously with coded redundancy.

   (2) No prior system applies RLNC across bearers with latency ratios
       exceeding 100:1.  US9537759B2 tested approximately 100:1
       (1500 ms versus 15 ms); HeMB operates at up to 900:1 (Iridium
       SBD 90 seconds versus ZigBee 100 milliseconds) within a single
       coded generation.

   (3) No prior system incorporates cost-weighted symbol allocation as
       a primary constraint on coded symbol distribution, ensuring
       that free bearers are exhausted before paid bearers receive any
       symbols.

---
