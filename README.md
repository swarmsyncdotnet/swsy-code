<p align="center"><img src="https://swarmsync.net/mascot.png" width="240" alt="Mel, the SwarmSync mascot: a bee boy with antennae in a black-and-yellow hoodie, in front of a honeycomb, syncing an upload folder and a download folder"><br><em>Mel the SwarmSync mascot</em></p>

# SwarmSync

Peer-to-peer file and folder sync between your own machines. A daemon,
`swarmsyncd`, and a TUI, `swarmsync`. Linux first, Android later.

**DESIGN STAGE** Nothing to download yet. SwarmSync is a design, subject to
total rewrite. This README represents the current state of the design as of
2026-11-17. Website:
[swarmsync.net](https://swarmsync.net/).

- **Transport:** QUIC on UDP 23420. TLS 1.3 with ChaCha20-Poly1305 only.
  Ed25519 identities, X25519 key agreement.
- **Trust:** one certificate chain per swarm: master code (24 words, printed
  once, the root key) → queens → workers. Queens admit and blacklist; every
  bee checks the chain.
- **Pairing:** trust on first use. Both screens show six words derived from
  the live connection, so a man in the middle can't make them match.
- **Data:** content-defined chunks (FastCDC) in a BLAKE3 tree, keyed per
  swarm. Every piece is verified on arrival; one root hash per share.
- **Root files:** privilege-separated like sshd.
  `sudo swarmsync add /etc/ssh/sshd_config` syncs it, permissions included.
- **Network:** a resolver that passes addresses and punches holes, LAN
  discovery, address gossip between bees. No relays in v1. Tailscale
  preferred when both ends run it.
- **Conflicts:** the newer change keeps the name; the other lands next to it
  as `notes.conflict-<swarm>-<bee>-<id>-<date>-<time>.txt`.
- **Config:** no file to edit. `swarmsync option set <name> <value>`.

Status: design phase. No code yet.

License: MPL-2.0 (see LICENSE).
