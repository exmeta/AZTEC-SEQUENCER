<h2 align="center">🛡️ Aztec Sequencer Node Guide (Testnet)</h2>

Aztec is building a privacy-first zk-rollup, and the **sequencer node** plays a critical role in this ecosystem. Running a node allows you to contribute early to the network and explore its infrastructure hands-on.

> ⚠️ **There is no official confirmation of rewards, airdrops, or incentives. This guide is for educational purposes and early contribution only.**

---

## 🖥️ System Requirements

| Component  | Recommended Specs           |
| ---------- | --------------------------- |
| CPU        | 8-core Processor            |
| RAM        | 16 GB                       |
| Storage    | 1 TB SSD                    |
| Connection | 25 Mbps (Upload & Download) |

> 💡 **Minimum to just try it out:** 4-core CPU, 6 GB RAM, and 25 GB SSD. But for longer uptime, recommended specs are needed to avoid crashes.

---

## 🌐 VPS

> 🎯 **Just want the `Apprentice` role on Aztec Discord?** You can run the node on **WSL** for \~30 minutes and get it—no VPS needed.

If you'd like to rent a VPS:
* [Contabo](https://contabo.com/en)
* [Hetzner](https://www.hetzner.com/cloud)

---

## ⚙️ Prerequisites

* **Sepolia RPC**: Use [Alchemy](https://dashboard.alchemy.com/apps) or [Infura](https://developer.metamask.io/register)
* **Beacon RPC**: Get it from [Chainstack](https://chainstack.com/global-nodes)
* **New EVM wallet** + **2.5 Sepolia ETH** (needed if you plan to register as validator)

> ⚠️ **Free plans have request limits.** Once reached, either upgrade or change your RPC URLs.

---

## 📥 Installation

1. Install `curl` and `wget` if they aren’t available:

```bash
command -v curl >/dev/null 2>&1 || apt-get update && apt-get install -y curl
command -v wget >/dev/null 2>&1 || apt-get install -y wget
```

2. Run either command below to install and launch the node:

```bash
[ -f "aztec.sh" ] && rm aztec.sh; curl -sSL -o aztec.sh https://raw.githubusercontent.com/zunxbt/aztec-sequencer-node/main/aztec.sh && chmod +x aztec.sh && ./aztec.sh
```

or

```bash
[ -f "aztec.sh" ] && rm aztec.sh; wget -q -O aztec.sh https://raw.githubusercontent.com/zunxbt/aztec-sequencer-node/main/aztec.sh && chmod +x aztec.sh && ./aztec.sh
```

---

## 🔧 Useful Commands

View node logs:

```bash
sudo docker logs -f --tail 100 $(docker ps -q --filter ancestor=aztecprotocol/aztec:latest | head -n 1)
```

Stop the node:

```bash
sudo docker stop $(docker ps -q --filter ancestor=aztecprotocol/aztec:latest | head -n 1)
```

---

## 🧩 Verify Node & Get `Apprentice` Role

> ⏱️ **Wait 10–20 minutes after starting the node before doing this step.**

1. Get your latest L2 block number:

```bash
curl -s -X POST -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","method":"node_getL2Tips","params":[],"id":67}' http://localhost:8080 | jq -r '.result.proven.number'
```

2. Use the block number to fetch your proof:

```bash
curl -s -X POST -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","method":"node_getArchiveSiblingPath","params":["block-number","block-number"],"id":67}' http://localhost:8080 | jq -r ".result"
```

3. Go to the `operators | start-here` channel in the [Aztec Discord](https://discord.com/invite/aztec)

4. Use this command:

```
/operator start
```

Provide:

* Your EVM wallet address
* The block number
* The proof from the command above

🎉 You’ll instantly get the `Apprentice` role.

---

## 🚀 Register as Validator

> ⚠️ If you see `ValidatorQuotaFilledUntil`, it means the daily limit is reached. Convert the given UNIX timestamp to local time and try again later.

Replace placeholders in the command below:

* `SEPOLIA-RPC-URL`
* `YOUR-PRIVATE-KEY`
* `YOUR-VALIDATOR-ADDRESS`

```bash
aztec add-l1-validator \
  --l1-rpc-urls SEPOLIA-RPC-URL \
  --private-key YOUR-PRIVATE-KEY \
  --attester YOUR-VALIDATOR-ADDRESS \
  --proposer-eoa YOUR-VALIDATOR-ADDRESS \
  --staking-asset-handler 0xF739D03e98e23A7B65940848aBA8921fF3bAc4b2 \
  --l1-chain-id 11155111
```

---

## 🤝 Contribute

Feel free to open an [issue](https://github.com/zunxbt/aztec-sequencer-node/issues) or submit a pull request to help improve this guide.

---
