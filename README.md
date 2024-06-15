# Aba-Splash

Aba-Splash! is a decentralized network for sharing [Offers](https://chialisp.com/offers/) across the [Aba](https://github.com/Aba-Network/aba-blockchain) ecosystem based on Rust's [libp2p](https://github.com/libp2p/js-libp2p) with DHT peer discovery. It is forked with permission from Dexie's Splash, created by them. Please see our Github repository to review the changes we have made from their code to adapt it to the Aba blockchain.

Every connected peer receives all offers broadcasted from other peers. There is no centralized connection, the peers connect to each other and are aware of each other.

The Splash! command line tools acts as a proxy between your application the Splash! network. It will broadcast your offers to the network and relay offers from other peers to your local application through a local HTTP API.

We're using a default port 11711 for Aba Offers w/ Aba-Splash.

## Installation

You can download prebuilt binaries in the
[releases section](https://github.com/Aba-Network/aba-splash/releases).

## Building

You can also build and install from source (requires the latest stable [Rust] compiler.)

```
cargo install --git https://github.com/Aba-Network/aba-splash.git aba-splash
```

## Usage

```
Usage: splash [OPTIONS]

Options:
  -k, --known-peer <MULTIADDR>
          Set initial peer, if missing use dexies DNS introducer
  -l, --listen-address <MULTIADDR>
          Set listen address, defaults to all interfaces, use multiple times for multiple addresses
  -i, --identity-file <IDENTITY_FILE>
          Store and reuse peer identity (only useful for known peers)
      --offer-hook <OFFER_HOOK>
          HTTP endpoint where incoming offers are posted to, sends JSON body {"offer":"offer1..."} (defaults to STDOUT)
      --listen-offer-submission <HOST:PORT>
          Start a HTTP API for offer submission, expects JSON body {"offer":"offer1..."}
  -h, --help
          Print help
```

## Examples

Start the node and listen on all interfaces (will use dexies DNS introducer):

`./aba-splash`

Start a node and open local webserver for offer submission on port 4000:

`./aba-splash --listen-offer-submission 127.0.0.1:4000`

Start a node and post incoming offers to a HTTP hook:

`./aba-splash --offer-hook http://yourApi/v1/offers`

Start a node and bootstrap from a known peer (will not use dexies DNS introducer):

`./aba-splash --known-peer /ip6/::1/tcp/12345/p2p/12D3K...`

Start a node and listen on a specific interface/port:

`./aba-splash --listen-address /ip6/::1/tcp/12345`

Start a node and reuse identity:

`./aba-splash --identity-file identity.json`

## Test the API with Docker

```bash
docker run -p 11711:11711 -p 4000:4000 dexiespace/splash:latest \
--listen-offer-submission 0.0.0.0:4000 \
--listen-address /ip4/0.0.0.0/tcp/11711
# send the request
curl -X POST -H "Content-Type: application/json" -d '{"offer":"offer1..."}' http://localhost:4000
```

## Become a stable peer

To become a stable peer, you need to open an inbound port in your firewall. Then start Splash! with the `--listen-address` option and choose your public interface and the selected port (eg. `11711`).

`./aba-splash --listen-address /ip6/2001:db8::1/tcp/11711 --listen-address /ip4/1.2.3.4/tcp/11711`

Running a stable peer? Let us know! We will add you to the default bootstrap list.
