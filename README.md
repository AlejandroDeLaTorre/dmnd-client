# DMND Stratum V2 Client – Getting Started Guide

## 1. Introduction

---

This guide walks you through setting up the DMND Stratum V2 Client and connecting to DMND pool.
After completing it, you will have a fully functional Stratum V2 mining setup connected to DMND
pool with full Job Declaration.

**How the components fit together:**

```
Bitcoin Core  →  Template Provider  →  DMND Client  →  Your Miners
(builds blocks)   (serves templates)   (manages work)   (hash)
```

## 2. What You Need Before Starting

---

To mine with DMND pool you must first obtain a DMND token. Please complete the registration form at
<https://onboarding.dmnd.work> and await our confirmation email before proceeding.

## 3. Enable Job Declaration Support

---

To use Stratum V2 with Job Declaration, you must run Bitcoin Core alongside the Stratum V2 Template
Provider. Job Declaration is one of the key features of Stratum V2 that allows miners to build
their own blocks, improving decentralization, efficiency, and latency.

What you need:

- Bitcoin Core (at least version 30) with IPC enabled.
- Stratum V2 Template Provider, which connects to Bitcoin Core via IPC and provides templates to
  the DMND Stratum V2 Client.

### 3.1 Run Bitcoin Core

Follow the instructions to download and install Bitcoin Core as described on the official website:

<https://bitcoincore.org/en/releases/30.2/>

Then start Bitcoin Core with IPC enabled:

```
bitcoin -m node -chain=main -ipcbind=unix
```

Note that `-ipcbind=unix` is required. Stratum V2 will not work without it.

### 3.2 Run Template Provider

Download the Template Provider binary:

<https://github.com/stratum-mining/sv2-tp/releases/tag/v1.0.6>

Run the Template Provider:

```
sv2-tp -debug=sv2 -loglevel=sv2:trace
```

If you changed Bitcoin Core's default datadir, you may need to specify the Unix socket path
manually by adding:

```
-ipcconnect=unix:<path-to-bitcoin-dir>/node.sock
```

The default Template Provider port is **8336**.

## 4. Run DMND Client

---

### 4.1 Download DMND Stratum V2 Client

Download the latest release for your platform from:

<https://github.com/dmnd-pool/dmnd-client/releases/latest>

> **Supported platforms:** Linux (x86_64). macOS and Windows are not currently supported.

### 4.2 Run the Client

Make the binary executable and run it:

```bash
chmod +x dmnd-client-linux

TOKEN=<DMND-token> ./dmnd-client-linux -l info -d <avg-hashrate>T --tp-address="127.0.0.1:<port>"
```

Where:

- `<DMND-token>` is the token you received via email from DMND pool during registration.
- `<avg-hashrate>` is the average hashrate of all your miners in TH/s. For example, if you have
  three machines at 100 TH/s, 200 TH/s, and 300 TH/s, the average is
  (100 + 200 + 300) / 3 = 200 TH/s. The dynamic difficulty adjustment algorithm handles the rest.
- `<port>` is the Template Provider listening port (default: **8336**).

**Example:**

```bash
TOKEN=abc123 ./dmnd-client-linux -l info -d 200T --tp-address="127.0.0.1:8336"
```

## 5. Connect Your Miners

---

Once Bitcoin Core, the Template Provider, and the DMND Client are all running, point your miners
at the machine running the DMND Client.

Use the following address (default port is **32767**):

```
stratum+tcp://<machine_running_dmnd_client_ip>:32767
```

Miner configuration:

- **Host/URL:** IP address of the machine running the DMND Client, port `32767`
- **Username:** can be left empty or set to anything
- **Password:** your DMND token

> **Firewall note:** If the DMND Client is running on a separate machine, make sure port `32767`
> is open and reachable from your miners.

## 6. Track Hashrate and Earnings

---

You can monitor your hashrate, shares, and earnings on the DMND pool dashboard:

```
https://dashboard.dmnd.work
```

Log in with the same credentials you used during registration.

Happy Mining!
