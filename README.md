Portscan
A lightweight, multithreaded TCP port scanner written in Python.

Portscan checks a predefined list of TCP ports on a target hostname or IPv4 address, reports ports that accept a TCP connection, and can save discovered open ports to a file. It is a small, hands-on project for learning socket programming, concurrency, command-line interfaces, and network reconnaissance.
<p align="left">
  <img src="https://img.shields.io/badge/Python-3.14%2B-3776AB?logo=python&logoColor=white" alt="Python 3.14+" />
  <img src="https://img.shields.io/badge/Protocol-TCP-informational" alt="TCP" />
  <img src="https://img.shields.io/badge/Execution-Multithreaded-success" alt="Multithreaded" />
  <img src="https://img.shields.io/badge/Platform-Linux-lightgrey?logo=linux&logoColor=white" alt="Linux" />
</p>

Use responsibly: scan only systems you own or have explicit permission to assess. Port scanning can generate logs and trigger security monitoring.

Features
- TCP connect scan using Python's socket module and connect_ex().
- Concurrent probing with a configurable number of worker threads.
- Two built-in port lists: a default Top 1K list and a larger list enabled with --top10k.
- Configurable connection timeout in milliseconds.
- Optional output file containing discovered open port numbers.
- Hostname or IPv4 target input.
- No third-party packages are required by the scanner's network-scanning logic.
Requirements
- Linux or another environment that supports Python TCP sockets.
- Python 3.14 or newer for the source as currently written, because it imports annotationlib.
The annotationlib import is not used elsewhere in this script. Removing from annotationlib import type_repr would allow the project to target older Python versions that support the remaining syntax (Python 3.10+).
Installation
git clone https://github.com/MessiasDev14011/Portscan.git
cd Portscan
python --version
No pip install step is needed for the current code; it uses the Python standard library.
Usage
Run the default scan against a hostname or IPv4 address:
python portscan.py --target scanme.nmap.org
The short form is also supported:
python portscan.py -t scanme.nmap.org
Common options
Option	Description	Default
-t, --target	Target hostname or IPv4 address (required)	—
--timeout	TCP connection timeout, in milliseconds	1000 ms
--threads	Number of worker threads	30
-o, --output	File to which open port numbers are written	No file
--top10k	Use the larger built-in port list instead of the default list	Disabled


Examples:
# Default list, default timeout and 30 threads
python portscan.py -t scanme.nmap.org

# Use a 2-second connection timeout
python portscan.py -t scanme.nmap.org --timeout 2000

# Use 50 worker threads
python portscan.py -t scanme.nmap.org --threads 50

# Scan the larger built-in port list
python portscan.py -t scanme.nmap.org --top10k

# Save discovered open ports to a file
python portscan.py -t scanme.nmap.org --output open-ports.txt

# Combine options
python portscan.py -t scanme.nmap.org --top10k --timeout 1500 --threads 40 -o open-ports.txt
Display all supported arguments with:
python portscan.py --help
Example output
The exact results depend on the target and can change over time. A run may look like this:
Domain = scanme.nmap.org
Timeout = 1000.0 ms
Threads = 30

[RESULTS]
[+]22   -> OPEN
[+]80   -> OPEN
[+]9929 -> OPEN

[#]3 Open Ports
[#]1000 Scanner Ports

Elapsed Time: 6.73 seconds
This is an illustrative example based on a test run, not a promise that the same ports will be open on every scan.
How it works
1. Parse arguments. Read the target, timeout, thread count, output path, and selected port list.
2. Build the work queue. Place the ports from the chosen built-in list into a thread-safe queue.
3. Probe ports concurrently. Worker threads create TCP sockets and attempt connections using connect_ex().
4. Record successful connections. A return code of 0 is treated as an open TCP port.
5. Report and optionally save results. Open ports are printed to the terminal and, when --output is supplied, written to the specified file.
Output file
When --output is enabled, the scanner writes one open port number per line:
22
80
9929
Note: the current implementation opens the output file in append mode. If the file already exists, results from a new run will be added to the existing contents. Use a new filename or clear the file before a run when you want a fresh result set.
Technical notes and limitations
- This is a TCP connect scanner, not a SYN/raw-packet scanner. It uses the operating system's normal TCP connection behavior.
- The current implementation targets IPv4 sockets (AF_INET).
- Only ports that successfully establish a TCP connection are reported as OPEN. Other outcomes are not currently classified separately as CLOSED or FILTERED.
- The scanner does not identify service names or versions, perform UDP scanning, or discover operating-system details.
- Results depend on network conditions, firewall rules, target availability, timeout settings, and thread count.
- Increasing concurrency can improve throughput but may also increase network load and timeouts.
- The predefined port lists are embedded in the source code; this version does not load a custom port list from a file.
- The reported scan-list size is based on the selected mode. The implementation should be validated against the actual list contents when changing those lists.
Roadmap
- [ ] Add support for a custom port range or port-list file.
- [ ] Add JSON output with target, timestamp, elapsed time, and open ports.
- [ ] Classify connection outcomes more precisely.
- [ ] Add unit tests and local integration tests.
- [ ] Add input validation for timeout and thread count.
- [ ] Add optional service-name lookup.
- [ ] Add progress reporting and a clearer scan summary.
Project purpose
Portscan is an educational project created to practice Python networking, TCP sockets, threading, queues, and building command-line tools for authorized security testing.
Legal and ethical use
Use this tool only on systems you own, isolated labs, platforms that explicitly permit testing, or targets for which you have written authorization. Do not use it to scan systems outside the approved scope.
