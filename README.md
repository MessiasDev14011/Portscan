Portscan

A lightweight, multithreaded TCP port scanner written in Python.

Portscan is a command-line tool designed to identify open TCP ports on a target hostname or IPv4 address. It uses Python's socket programming capabilities and multithreading to perform concurrent connection checks.

This project was built as a hands-on exploration of network reconnaissance, TCP connections, concurrency, and Python development.

Features

- TCP Connect Scanning — identifies ports that accept TCP connections using "socket.connect_ex()".
- Multithreading — scans multiple ports concurrently using configurable worker threads.
- Predefined Port Lists — choose between the default port list and an extended list with "--top10k".
- Configurable Timeout — adjust the TCP connection timeout in milliseconds.
- Result Export — save discovered open port numbers to a text file.
- Command-Line Interface — configure scans through straightforward CLI arguments.
- No Third-Party Dependencies — uses Python's standard library.

Requirements

- Python 3.14 or newer for the current source code, due to its "annotationlib" import.

«Note: "annotationlib" is imported but not used by the scanner. Removing that import would allow compatibility with older Python versions supported by the remaining code.»

Installation

Clone the repository:

git clone https://github.com/MessiasDev14011/Portscan.git
cd Portscan

Check your Python version:

python --version

No dependency installation is required for the current implementation.

Usage

Run a scan against a hostname or IPv4 address:

python portscan.py --target scanme.nmap.org

You can also use the short form:

python portscan.py -t scanme.nmap.org

Command-Line Options

Option| Description| Default
"-t", "--target"| Target hostname or IPv4 address| Required
"--timeout"| TCP connection timeout in milliseconds| "1000"
"--threads"| Number of worker threads| "30"
"--top10k"| Use the extended predefined port list| Disabled
"-o", "--output"| File path for saving open port numbers| Disabled
"-h", "--help"| Display help information| —

Examples

Use the default port list:

python portscan.py -t scanme.nmap.org

Set a 2-second connection timeout:

python portscan.py -t scanme.nmap.org --timeout 2000

Increase the number of worker threads:

python portscan.py -t scanme.nmap.org --threads 50

Use the extended port list:

python portscan.py -t scanme.nmap.org --top10k

Save discovered open ports:

python portscan.py -t scanme.nmap.org -o open-ports.txt

Combine options:

python portscan.py -t scanme.nmap.org \
  --top10k \
  --timeout 1500 \
  --threads 40 \
  -o open-ports.txt

Display all available arguments:

python portscan.py --help

Example Output

The following is an illustrative example. Results and execution time depend on the target and network conditions.

Domain = scanme.nmap.org
Timeout = 1000.0 ms
Threads = 30

[RESULTS]
[+] 22 -> OPEN
[+] 80 -> OPEN
[+] 9929 -> OPEN

[#] 3 Open Ports
[#] 1000 Scanner Ports

Elapsed Time: 6.73 seconds

How It Works

1. Argument Parsing — reads the target, timeout, thread count, output path, and selected port list.
2. Work Queue — places the selected ports into a thread-safe queue.
3. Concurrent Scanning — worker threads create TCP sockets and attempt connections using "connect_ex()".
4. Result Collection — ports returning a successful connection result are classified as open.
5. Output Generation — displays discovered open ports and optionally saves their numbers to a text file.

Technical Details and Limitations

- Uses TCP connect scanning rather than SYN or raw-packet scanning.
- Currently supports IPv4 sockets.
- Reports ports that successfully establish a TCP connection as "OPEN".
- Does not separately classify other outcomes as "CLOSED" or "FILTERED".
- Does not perform UDP scanning, service version detection, operating system fingerprinting, or vulnerability detection.
- Uses predefined port lists embedded in the source code; custom port ranges and external port-list files are not currently supported.
- Output files are opened in append mode, so existing results are preserved and new results are added.
- Scan performance and results depend on network conditions, firewall rules, timeout settings, and thread count.

Increasing the number of threads may improve scan speed, but excessive concurrency can increase network load and lead to timeouts.

Roadmap

- [ ] Add custom port ranges.
- [ ] Support loading port lists from a file.
- [ ] Add JSON output with scan metadata.
- [ ] Improve input validation and error handling.
- [ ] Add automated tests.
- [ ] Improve progress reporting and scan statistics.

Responsible Use

This tool is intended for educational purposes and authorized network assessments.

Only scan systems you own or have explicit permission to assess. Port scanning may generate logs, trigger security alerts, and be subject to applicable laws and organizational policies.

Author

Miguel — MessiasDev14011

- GitHub: "@MessiasDev14011" (https://github.com/MessiasDev14011)
- Project: "Portscan" (https://github.com/MessiasDev14011/Portscan)

---

Built to learn, experiment, and improve Python programming and network security skills.