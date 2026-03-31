## TASK 7 — ISE COVER LETTER

To: rfc-ise@rfc-editor.org
Subject: Independent Submission — draft-papadopoulos-hemb-00

Dear Eliot,

Following our correspondence in March 2026 regarding
draft-papadopoulos-ipougrs-00, I am submitting a related document for
consideration as an Informational RFC on the Independent Submission
Stream.

draft-papadopoulos-hemb-00 specifies HeMB (Heterogeneous Media
Bonding), a sub-IP protocol that bonds multiple physically incompatible
communication bearers into a single virtual bearer using Random Linear
Network Coding (RLNC).  The protocol addresses the specific problem of
reliable message delivery when the only surviving channels are raw
physical bearers — LoRa mesh radio, Iridium SBD satellite, APRS/AX.25
amateur radio, cellular SMS, and GSM ring signals — that share no
common protocol stack and differ in latency by up to three orders of
magnitude.

I believe this document is appropriate for the Independent Submission
Stream because no existing IETF working group covers sub-IP
heterogeneous bearer bonding.  MPTCP (tsvwg), QUIC (quic), DTN (dtn),
and RLNC-related research all operate above IP or within a single
technology.  HeMB operates below IP by design — it is consumed by
routing and transport layers rather than competing with them.

I want to be explicit about what is and is not novel.  RLNC is not
novel (Ho et al., 2006).  Any-K-of-N recovery is not novel (RFC 5053).
RLNC across heterogeneous paths is not novel (US Patent 9,537,759).
HeMB's contribution is the specific combination: sub-IP operation on
physically incompatible bearers with no shared stack, RLNC across
latency ratios exceeding 100:1 (up to 900:1 in practice), and
cost-weighted symbol allocation.  No prior system addresses this
combination — confirmed by three independent prior art research passes.

The protocol has a production-quality open-source reference
implementation (GPLv3) across three codebases: Go bridge (4,314
lines), Go hub (756 lines), and Kotlin Android (1,871 lines), with
65 tests passing across all three.  End-to-end decoding has been
validated on production hardware: 246 RLNC generations decoded with
zero failures, sub-millisecond P50 decode latency.  Field validation
with LoRa + Iridium SBD + SMS bearers is scheduled for April 2026.

I hold no patents related to HeMB and have no patents pending.  The
reference implementation is licensed under GPLv3.

I would appreciate Informational track designation.  The document
describes a working protocol with implementation evidence but does not
propose changes to existing IETF protocols.

Respectfully,
Kyriakos Papadopoulos
kyriakos@papadopoulos.tech
