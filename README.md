# Provenance

Public commitments to Will Glynn's private dialogue archive: his conversations with Jarvis on
philosophy, ethics, religion, economics, society and architecture. Nothing here reveals their content.
Each commitment fixes the archive's state at a date, so any exchange revealed later can be shown to have
existed by then. This is commit-reveal publishing: keep the text private, publish the proof of it.

## Files
- `chain.txt`: one line per archived exchange, oldest first: `<content hash> <chain hash>`.
  The content hash is SHA-256 of the exchange. Each chain hash is SHA-256 of (the previous chain hash,
  as hex, followed by this content hash); the first one uses an empty previous hash.
- `heads.jsonl`: the last chain hash for each day, with the entry count.
- `heads/DATE.txt` and `heads/DATE.txt.ots`: that day's head and its OpenTimestamps proof, which anchors
  the head in the Bitcoin blockchain once a block confirms it (verify with `ots verify heads/DATE.txt.ots`).
- `revealed/<content hash>.json`: exchanges published on purpose, on their scheduled reveal date.
- `allowed_signers`: the public key that signs every commit here.

## Verifying a revealed exchange
1. Take the `body` object, serialize it as JSON with keys sorted and non-ASCII characters kept as they
   are, and hash it with SHA-256. It must equal the file name.
2. Find that hash in `chain.txt` and recompute the chain hashes from there to the end. The last one must
   match a head in `heads.jsonl`.
3. That head's `.ots` file proves the date; the commit signature (`git verify-commit` with
   `allowed_signers`) shows who published it.

## What this does not prove
- That no one had the idea earlier: only that this text existed by the head's date.
- Prior art: a hash does not stop anyone from patenting an idea; only publishing the text does. That is
  what the scheduled reveals are for.
- GitHub commit dates can be set by the author; the date evidence is the OpenTimestamps proof.
