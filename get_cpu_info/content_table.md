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

### 🔧 Usage:
```bash
lscpu
```
---

## 1. `nproc --all`
