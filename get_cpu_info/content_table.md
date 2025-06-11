# :computer: Viewing CPU Core Information in Linux CLI

Understanding CPU's core and architecture information for system diagnostics, performance tuning, and capacity planning. Linux command-line tools to extract detailed information about CPU.

View CPU core and architecture details in a Linux environment.

---

## :pushpin: Table of Contents

1. [lscpu](#1-lscpu)
2. [nproc --all](#2-nproc---all)
3. [cat /proc/cpuinfo](#3-cat-proccpuinfo)
4. [dmidecode -t processor](#4-dmidecode--t-processor)
5. [top or htop](#5-top-or-htop)
6. [Summary Table](#6-summary-table)
7. [Conclusion](#7-conclusion)

---

## 1. `lscpu`

The `lscpu` command displays an overview of the CPU architecture. It summarizes the CPU layout including:

- Number of CPUs
- Threads per core
- Cores per socket
- Number of sockets
- NUMA nodes (if applicable)

### :wrench: Usage:
```bash
lscpu
```
---
## 2. `nproc --all`

Shows the total number of **logical processors**, including hyper-threaded cores.

- Run `nproc` without `--all` to see only the processors available to the current process.

### :wrench: Usage:
```bash
nproc --all
```
---
## 3. `cat /proc/cpuinfo`

This command displays detailed information about each CPU core, including:
- Processor ID
- Vendor ID
- Model name
- CPU family
- Cache size
- Flags (features)

### :wrench: Usage:
```bash
cat /proc/cpuinfo
```
Tip: Use grep to filter specific fields:
```bash
cat /proc/cpuinfo | grep "model name"

```
---
## 4. `dmidecode -t processor`

The dmidecode command retrieves hardware-level information from the system's DMI (Desktop Management Interface) table.
- Use the -t processor flag to get CPU-specific data.
- Requires superuser privileges.

### :wrench: Usage:
```bash
sudo dmidecode -t processor
```
---
## 5. `top or htop`

These are interactive tools used for monitoring CPU usage and system performance in real time.

- top is included by default on most Linux distributions.
- htop offers a user-friendly and colorful interface (may require installation).

### :wrench: Usage:
```bash
top 
```
```bash
htop
```
> Install htop if not available:

```bash
sudo apt install htop   # Debian/Ubuntu
sudo yum install htop   # CentOS/RHEL
```
---
## 6. `Summary Table`

| **Command** | **description** | <br>
| `lscpu` | Overview of CPU architecture and topology | <br>
| `nproc --all` | Total logical processors (including hyper-threading) | <br>
| `cat /proc/cpuinfo` | Detailed per-core CPU information | <br>
| `dmidecode -t processor` | Hardware-level CPU information | <br>
| `top / htop` | Real-time CPU usage and process viewer | <br>

---
## 7. `Conclusion`

#### These commands offer different levels of detail to help you understand the CPU layout and performance characteristics of your Linux system. Use them individually or in combination depending on your needs—whether you're checking basic specs, debugging, or optimizing your server's performance.
---