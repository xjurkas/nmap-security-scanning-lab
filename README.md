# Nmap Security Scanning Lab

A practical semester project focused on **Nmap (Network Mapper)** and its use in network discovery, security auditing, service enumeration, operating-system detection, Nmap Scripting Engine scans, and firewall behavior analysis.

The project combines a technical overview of Nmap with reproducible experiments performed only in controlled environments: a private home network and an intentionally vulnerable Metasploitable 2 virtual machine.

> [!WARNING]
> Use Nmap only on systems and networks that you own or are explicitly authorized to test. Unauthorized scanning may violate laws, policies, or terms of service.

## Project objectives

The main objectives of the project are to:

- explain how Nmap works and where it is used,
- demonstrate installation on Windows, Linux, and macOS,
- introduce the Zenmap graphical interface,
- perform host discovery and port scanning,
- identify running services and software versions,
- estimate the operating system of a target host,
- use Nmap Scripting Engine scripts for discovery and vulnerability assessment,
- compare scan results before and after firewall filtering,
- evaluate Nmap in terms of functionality, performance, accuracy, and usability.

## Covered Nmap features

| Area | Covered functionality |
| --- | --- |
| Host discovery | Ping sweeps and identification of active hosts |
| Port scanning | TCP SYN, TCP Connect, FIN, Xmas, and basic TCP scans |
| Service detection | Identification of services and software versions with `-sV` |
| OS detection | OS fingerprinting with `-O` and `--osscan-guess` |
| NSE | Discovery, vulnerability, and SSH authentication-testing scripts |
| Firewall analysis | Comparison of scan results before and after firewall rules |
| Reporting | Human-readable and structured Nmap output formats |
| GUI | Basic usage of the Zenmap graphical interface |

## Experiments

### 1. Host discovery and basic port scanning

Active hosts were discovered in a private `/24` network, followed by a basic port scan of a selected host.

```bash
nmap -sn 192.168.1.0/24
nmap 192.168.1.40
```

The experiment demonstrated how Nmap can identify reachable devices and determine which TCP ports are open, closed, or filtered.

### 2. Service and version detection

The `-sV` option was used to identify applications listening on open ports and estimate their versions.

```bash
nmap -sV 192.168.1.40
```

This information is useful when checking for outdated, unnecessary, or potentially vulnerable services.

### 3. Operating-system detection

Nmap OS fingerprinting was tested with standard detection and the more permissive guessing mode.

```bash
nmap -O 192.168.1.40
nmap -O --osscan-guess 192.168.1.40
```

The target was identified as a Windows-based system with a high-confidence match, although filtering reduced the reliability of the fingerprint.

### 4. Nmap Scripting Engine

NSE experiments were performed against a deliberately vulnerable Metasploitable 2 virtual machine.

```bash
nmap -sV 192.168.56.101
nmap --script discovery 192.168.56.101
nmap --script vuln 192.168.56.101
nmap --script ssh-brute -p 22 192.168.56.101
```

The scans demonstrated service enumeration, additional host discovery, vulnerability detection, and controlled authentication testing. The vulnerability scan identified the known **vsFTPd 2.3.4 backdoor** on the lab machine.

### 5. Firewall and scan-behavior comparison

Several scan types were compared before and after restrictive firewall rules were enabled.

```bash
nmap -sF -p 1-1000 192.168.56.101
nmap -sS -p 1-1000 192.168.56.101
nmap -sT -p 1-1000 192.168.56.101
nmap -sX -p 1-1000 192.168.56.101
```

Before filtering, SYN and TCP Connect scans detected multiple open services. After the firewall rules were applied, ports were generally reported as `filtered` or `open|filtered`, showing how strongly scan results depend on packet-filtering behavior.

## Installation

### Windows

Download and run the official Nmap installer. Keep **Npcap** selected during installation because it provides the packet-capture functionality required by advanced scans.

Verify the installation:

```powershell
nmap --version
```

### Debian and Ubuntu

```bash
sudo apt update
sudo apt install nmap
nmap --version
```

### Fedora

```bash
sudo dnf install nmap -y
nmap --version
```

### Arch Linux and Manjaro

```bash
sudo pacman -S nmap
nmap --version
```

### macOS with Homebrew

```bash
brew install nmap
nmap --version
```

Some scan types require administrator or root privileges because they use raw network packets.

## Zenmap

Zenmap is a graphical front end for Nmap. It provides:

- target and scan-profile selection,
- real-time command output,
- discovered host and port lists,
- service information,
- graphical network topology visualization.

The project introduces Zenmap as a beginner-friendly interface, while the experiments use the command-line version of Nmap for greater control and reproducibility.

## Selected command reference

| Command | Purpose |
| --- | --- |
| `nmap -sn <network>` | Discover active hosts without a port scan |
| `nmap <target>` | Scan the most common TCP ports |
| `nmap -sS <target>` | Perform a TCP SYN scan |
| `nmap -sT <target>` | Perform a full TCP Connect scan |
| `nmap -sU <target>` | Scan UDP ports |
| `nmap -sV <target>` | Detect services and versions |
| `nmap -O <target>` | Estimate the target operating system |
| `nmap -A <target>` | Enable OS detection, version detection, scripts, and traceroute |
| `nmap -sC <target>` | Run the default NSE script set |
| `nmap --script discovery <target>` | Run discovery-category NSE scripts |
| `nmap --script vuln <target>` | Run vulnerability-detection NSE scripts |
| `nmap -p- <target>` | Scan all TCP ports |
| `nmap -oA <name> <target>` | Save results in multiple output formats |

## Repository structure

```text
.
├── Nmap.pdf    # Full project report, screenshots, commands, and evaluation
└── README.md   # Repository overview
```

## Key findings

- Nmap provides effective host discovery and port scanning in small and medium-sized networks.
- Service detection provides valuable context beyond a simple list of open ports.
- OS fingerprinting becomes less reliable when firewalls filter or normalize traffic.
- NSE significantly extends Nmap through automated discovery and vulnerability checks.
- Firewall rules can change results from clearly identified ports to `filtered` or `open|filtered` states.
- No scan method is guaranteed to bypass a correctly configured firewall.
- The command-line interface provides more control than Zenmap but requires greater familiarity with Nmap options.

## Documentation

The complete report is available in [`Nmap.pdf`](Nmap.pdf). It contains:

- installation instructions for major operating systems,
- an overview of Zenmap,
- explanations of core Nmap functions,
- command examples,
- screenshots from all experiments,
- experiment conclusions,
- a final evaluation of Nmap.

## Technologies and environment

- Nmap
- Nmap Scripting Engine (NSE)
- Zenmap
- Npcap
- Metasploitable 2
- Oracle VirtualBox
- Windows and Linux test environments

## Author

**Dominik Jurkas**  
Faculty of Informatics and Information Technologies  
Slovak University of Technology in Bratislava

## License

This repository is intended for educational and authorized security-testing purposes. The included report remains the work of its author. Third-party tools and software are governed by their respective licenses.
