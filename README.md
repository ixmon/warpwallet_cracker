# warpwallet_cracker
A brute-force cracker in Go for the WarpWallet Challenge 2: https://keybase.io/warp/

# Usage

```
$ go get ./...
---
$ go build warpwallet_cracker.go
---
$ ./run.sh 
Using address "1MkupVKiCik9iyfnLrJoZLx9RH4rkF3hnA" and salt "a@b.c"
Tried 4 passphrases in 2.269448485s [last passphrase: 2zZM3L1C]

You may also supply a 3rd parameter directly to the warpwallet executable to specify a single passphrase to try
warpwallet_cracker 1MkupVKiCik9iyfnLrJoZLx9RH4rkF3hnA a@b.c nakamoto
This will crack the address 1MkupVKiCik9iyfnLrJoZLx9RH4rkF3hnA using the salt a@b.c with the passphrase nakamoto.
Omitting this parameter will cause the cracker to try random passphrases 8 characters in length built upon lower and upper case alphanumerics.
```

# Performance
This script has been optimized for speed. On a MacBook Pro this achieves ~1.1 hash/sec/core (with hyperthreading enabled). At this hashrate it is [not feasible](https://www.wolframalpha.com/input/?i=(62%5E8+%2F+1.1)+seconds+to+years) to enumerate the entire keyspace of 62^8 hashes.

For example: if you run this script for a year on a quad-core Macbook, there is a [1 in ~37M](https://www.wolframalpha.com/input/?i=62%5E8+%2F+(3600+*+365+*+1.1+*+4)) chance of your RNG gifting you 20 BTC.

Ideas for further improvements:
- Explore using SSE2 for faster scrypt
- Use all cores of the CPU
- Build a bloom filter containing past attempts (is a bloom filter lookup faster than a hash attempt?)
- Figure out a way to share the bloom filter with other crackers
- Deterministic key generation for better search space partitioning (instead of seeding rand with a unix timestamp)

# How-to
Build:

`go build warpwallet_cracker.go`

Params:

`./warpwallet_cracker [Address] [Salt - optional]`

Run (Windows):

`run.bat`

Run (*nix):

`./run.sh`

Run unit tests and benchmark:

`go test -bench=.`
