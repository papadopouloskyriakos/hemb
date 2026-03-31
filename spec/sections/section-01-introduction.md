## TASK 1 — SECTION 1: INTRODUCTION

### 1.1.  Problem Statement

   Field communication in disaster response, maritime, polar, and
   contested environments frequently degrades to a patchwork of
   surviving physical bearers: Meshtastic LoRa mesh radios on 868/915
   MHz, Iridium Short Burst Data (SBD) satellite modems, APRS over
   AX.25 amateur radio, native cellular SMS, and in extremis, IP over
   Unanswered GSM Ring Signals (IPoUGRS) [IPOUGRS2026] at 1.6 bits
   per second.  These bearers share no common protocol stack.  They
   differ in maximum transmission unit (MTU) by five orders of
   magnitude — from 1 byte (IPoUGRS) to 100 kilobytes (Iridium IMT).
   They differ in one-way latency by three orders of magnitude — from
   100 milliseconds (ZigBee, LoRa) to 90 seconds (Iridium SBD).  They
   differ in per-message cost by an unbounded factor — from zero (LoRa,
   APRS) to approximately EUR 0,05 per message (Iridium SBD).

   Existing multipath transport protocols — MPTCP [RFC8684], MP-QUIC
   [RFC9000], GRE Tunnel Bonding [RFC8157] — require IP connectivity on
   every subflow.  When IP infrastructure is destroyed, jammed, or
   never existed, these protocols are inapplicable.  Link aggregation
   protocols such as LACP [IEEE802.1AX] assume homogeneous Ethernet
   links.  Delay-tolerant overlay protocols such as the Bundle Protocol
   [RFC9171] provide store-and-forward delivery but do not bond bearers
   simultaneously with coded redundancy.

   The problem, stated precisely, is: how to present a single reliable
   virtual bearer to a routing or application layer above, when the
   physical bearers below are physically incompatible, share no common
   framing or addressing, have radically different costs, and differ in
   latency by up to three orders of magnitude.

### 1.2.  Approach

   HeMB bonds N physical bearers into one virtual bearer.  Each bearer
   is abstracted as a dumb pipe with only two operations: send a byte
   sequence and receive a byte sequence.  No IP, TCP, UDP, or shared
   framing is required.

   Payloads are segmented into K source segments, then encoded using
   Random Linear Network Coding (RLNC) [HO2006] over GF(256) into N
   coded symbols (N >= K).  Each coded symbol is a random linear
   combination of all K source segments.  Symbols are distributed across
   bearers using cost-weighted allocation: free bearers (LoRa, APRS)
   are exhausted before paid bearers (Iridium SBD, cellular SMS)
   receive any symbols.  Per-bearer repair symbol counts are adaptive,
   based on measured bearer loss rates.

   The receiver collects coded symbols from any bearer combination.
   When K linearly independent symbols are received — regardless of
   which bearers delivered them, in what order, or with what latency —
   Gaussian elimination over GF(256) recovers the original K source
   segments.  Bearer identity is irrelevant to decoding.

   HeMB is routing-protocol agnostic.  It presents a single
   Send(payload)/Receive() interface upward.  This interface may be
   consumed by Reticulum [MESHSAT2026], by an IP stack via a TUN/TAP
   virtual network interface, by the Bundle Protocol [RFC9171], or
   directly by an application.

   When only one bearer is available (N=1), HeMB degrades to direct
   passthrough with zero protocol overhead — no RLNC encoding, no frame
   header, no additional latency.  The raw payload is delivered on the
   single bearer unchanged.

### 1.3.  Related Work

   (See Task 2 below.)

### 1.4.  Requirements Language

   The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
   "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and
   "OPTIONAL" in this document are to be interpreted as described in
   BCP 14 [RFC2119] [RFC8174] when, and only when, they appear in all
   capitals, as shown here.

---
