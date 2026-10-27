<div align="center">

# Portscan

### A lightweight, multithreaded TCP port scanner built with Python

A small command-line tool for checking TCP port availability using Python sockets, a port queue, and concurrent worker threads.

<br>

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Networking](https://img.shields.io/badge/Networking-TCP-0B7285?style=for-the-badge)
![Concurrency](https://img.shields.io/badge/Concurrency-Threads-6C5CE7?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Linux-lightgrey?style=for-the-badge&logo=linux&logoColor=white)

[Features](#-features) · [Installation](#-installation) · [Usage](#-usage) · [Options](#-command-line-options) · [How It Works](#-how-it-works) · [Limitations](#-current-scope-and-limitations)

</div>

---

## Overview

**Portscan** is a command-line TCP port scanner developed as a hands-on Python and networking project. It checks a predefined list of commonly used ports and reports those that accept TCP connections.

The project focuses on learning how socket connections, timeouts, queues, and multithreading can be combined to perform network checks efficiently.

> **Project status:** A learning project under active development. It is not intended to replace mature tools such as Nmap.

## ✨ Features

- **TCP connect scanning** using Python's `socket` module.
- **Concurrent workers** to test multiple ports at the same time.
- **Preset port lists** for a smaller scan and an expanded scan.
- **Configurable timeout** for each connection attempt.
- **Configurable thread count** to control concurrency.
- **Optional output file** that records discovered open port numbers.
- **Terminal output** that displays open ports and scan duration.

## 🧰 Built With

| Technology | Purpose |
| --- | --- |
| Python | Main programming language |
| `socket` | TCP connection attempts |
| `threading` | Concurrent scanning workers |
| `queue` | Distributes ports to workers |
| `argparse` | Command-line argument parsing |

The scanner uses Python's standard library; no third-party package is required by the current version of the code.

## 🚀 Installation

### Requirements

- Python **3.10 or newer**
- Git (optional, for cloning the repository)

### Clone the repository

```bash
git clone https://github.com/MessiasDev14011/Portscan.git
cd Portscan
```

### Optional: create a virtual environment

```bash
python -m venv .venv
source .venv/bin/activate
```

There are currently no third-party dependencies to install. Run the tool directly with Python:

```bash
python portscan.py --help
```

## 💻 Usage

The target is required. It can be a hostname or an IPv4 address.

### Scan the default port list

```bash
python portscan.py --target 192.0.2.10
```

Short form:

```bash
python portscan.py -t 192.0.2.10
```

`192.0.2.10` is an address reserved for documentation; replace it with a host you own or are authorized to assess.

### Set the connection timeout

Timeouts are specified in **milliseconds**. The default is `1000` ms.

```bash
python portscan.py -t 192.0.2.10 --timeout 1500
```

### Change the number of threads

The default is `30` worker threads.

```bash
python portscan.py -t 192.0.2.10 --threads 50
```

### Use the expanded port list

```bash
python portscan.py -t 192.0.2.10 --top10k
```

### Save discovered open ports

```bash
python portscan.py -t 192.0.2.10 --output results.txt
```

The current implementation writes **one open port number per line**. It opens the file in append mode, so results from later runs are added to the existing file rather than replacing it.

### Combine options

```bash
python portscan.py \
  --target 192.0.2.10 \
  --timeout 1200 \
  --threads 40 \
  --output results.txt
```

## ⚙️ Command-Line Options

| Argument | Required | Default | Description |
| --- | :---: | --- | --- |
| `-t`, `--target` | Yes | — | Target hostname or IPv4 address |
| `--timeout` | No | `1000` ms | TCP connection timeout, in milliseconds |
| `--threads` | No | `30` | Number of worker threads |
| `-o`, `--output` | No | Not saved | Append discovered open port numbers to a file |
| `--top10k` | No | Disabled | Use the expanded port list instead of the default list |
| `-h`, `--help` | No | — | Display command-line help |

## 🖥️ Example Output

Output varies depending on the target and its network configuration. A typical run may look like this:

```text
Domain = 192.0.2.10
Timeout = 1000.0 ms
Threads = 30

[RESULTS]
[+]22   -> OPEN
[+]80   -> OPEN

[#]2 Open Ports
[#]1000 Scanner Ports

Elapsed Time: 1.42 seconds
```

The addresses and results above are illustrative examples, not results from a real scan.

## 🔍 How It Works

1. **Parse arguments** — `argparse` reads the target and optional settings.
2. **Select ports** — the scanner loads its default or expanded predefined port list.
3. **Queue work** — each port is added to a thread-safe queue.
4. **Start workers** — worker threads take ports from the queue and attempt TCP connections.
5. **Check connection result** — a successful connection (`connect_ex()` returns `0`) is reported as open.
6. **Record findings** — open port numbers are stored in memory and optionally appended to a text file.
7. **Summarize** — the program prints the number of detected open ports and elapsed time.

### Scan method

This project uses a **TCP connect scan**. It asks the operating system to establish a normal TCP connection, rather than crafting raw packets. As a result, the current implementation does not require raw-socket privileges just to perform its TCP connection checks.

## 📌 Current Scope and Limitations

The current version is intentionally simple. It:

- checks TCP ports only;
- uses predefined port lists rather than accepting arbitrary ranges or custom list files;
- reports whether a TCP connection succeeds, but does not identify services or versions;
- does not perform UDP scanning, OS detection, banner grabbing, or vulnerability detection;
- records only open port numbers in the optional output file;
- appends to an existing output file instead of overwriting it;
- does not currently provide structured JSON or HTML reports.

A port that does not appear as open is **not necessarily closed**: timeouts, filtering, routing, and the target's current state can affect the result.

## 🗺️ Roadmap

Potential improvements for future versions:

- [ ] Add support for custom port ranges and port-list files.
- [ ] Validate the target, timeout, and thread count before scanning.
- [ ] Add clearer progress and error reporting.
- [ ] Add an option to overwrite output files safely.
- [ ] Export structured results as JSON.
- [ ] Add automated tests for connection results and argument handling.
- [ ] Add optional service identification as a separate stage.

## 🔐 Responsible Use

Use Portscan only against systems you own or have explicit permission to test. Scanning may generate security alerts and can be restricted by network policy. Confirm authorization and scope before running scans against any external host.

## 👤 Author

**MessiasDev14011**


<div align="center">

**Built as a practical project to strengthen Python, networking, and cybersecurity fundamentals.**

</div>
