# 403‑Killchain

<p align="center">
  <img src="https://raw.githubusercontent.com/Arc-Cyber-Arsenal/403-Killchain/master/killchain.png" alt="403-Killchain Banner" width="600">
</p>

[![Language](https://img.shields.io/badge/Go-1.20+-00ADD8?logo=go)](https://go.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform](https://img.shields.io/badge/platform-windows%20%7C%20linux%20%7C%20macos-lightgrey)](https://github.com/Archsec-Emman/403-Killchain)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

**Turn 403 Forbidden into 200 OK — One command, one killchain.**  
Four independent bypass engines, fused into a single, silent Go binary. Each engine uses a unique strategy. They run **in parallel**, and the results are merged into one clear, actionable report. No more running multiple tools, no more missed tricks.

---

## 📖 Overview

403‑Killchain is a penetration testing tool designed to help security professionals bypass HTTP 403 Forbidden errors. It works by embedding four distinct bypass engines, each attacking the same target URL with its own set of techniques. The engines run concurrently, meaning you get the collective output in less time than it would take to run two of them sequentially.

The tool is written in Go, compiled into a single static binary with **no external runtime dependencies**, and works offline — no API calls, no internet required.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| **4 engines, 1 binary** | No external dependencies at runtime — everything is compiled in. |
| **⚡ Parallel execution** | All four engines attack the target simultaneously. |
| **🔧 Engine selection** | Use `--only` or `--skip` to run specific engines. |
| **📋 Unified report** | Clear, sectioned output for easy analysis. |
| **📦 Embedded payloads** | Every bypass technique is compiled directly into the binary. |
| **🖥️ Static binary** | Works on Linux, Windows, and macOS — no installers required. |
| **🔌 Zero external API calls** | Runs entirely offline. |

---

## 🧠 How It Works

403‑Killchain embeds four distinct engines, each attacking the same URL with its own set of techniques.

### Engine 1 – Path & Header Fuzzer
This engine systematically mutates the URL path and injects common bypass headers.

- Appends slashes, encoded characters, and path traversal sequences (`/`, `%2e`, `;`, `..;/`)
- Adds headers like `X-Forwarded-For`, `X-Originating-IP`, `X-Rewrite-URL`, `X-Real-IP`
- Switches HTTP methods on the fly (GET → POST, PUT, PATCH, DELETE, OPTIONS)
- Sends every combination quickly and reports any response that isn’t a `403`

### Engine 2 – Header & Method Mutator
This engine takes a more protocol‑oriented approach, manipulating the request line and headers to confuse access controls.

- Inserts malformed request lines (HTTP/1.0 downgrades, extra spaces, double slashes)
- Tries alternative host headers, `X-Forwarded-Host`, `Origin` overrides
- Exploits discrepancies between front‑end proxies and back‑end servers
- Uses payloads designed to bypass WAFs and reverse proxies

### Engine 3 – Status‑Code Reasoner
This engine brute‑forces the common patterns that web servers use to enforce `403` rules.

- Tests every known HTTP method with different Content‑Types
- Sends the same request multiple times with small changes (case‑flipping, Unicode tricks)
- Learns from `nmap`‑style path lists and common admin panels
- Flags any response that silently returns `200`, `301`, or `302`

### Engine 4 – Encoding & Randomisation
This engine uses entropy to avoid pattern‑based blocking.

- URL‑encodes, double‑encodes, and Unicode‑encodes every path segment
- Generates random parameter names and values to evade signature‑based filters
- Sends requests at slightly varied speeds (jitter) to stop rate‑limiting
- Combines multiple bypass techniques into single, high‑entropy requests

All four engines run **concurrently** — you get the full collective output in less time than it takes to run two of them separately.

---

## 🚀 Installation

### Option 1 — Pre‑compiled Binary (Recommended)
Download the latest binary for your OS from the [Releases](https://github.com/Archsec-Emman/403-Killchain/releases) page.

| Platform | Download |
|----------|----------|
| **Windows** | `403-Killchain-windows-amd64.exe` |
| **Linux** | `403-Killchain-linux-amd64` |
| **macOS** | `403-Killchain-darwin-amd64` |

### Option 2 — Build from Source (Go 1.20+ required)

```bash
git clone https://github.com/Archsec-Emman/403-Killchain.git
cd 403-Killchain
CGO_ENABLED=0 go build -o 403-Killchain
```

The `CGO_ENABLED=0` flag produces a fully static binary with no external dependencies, suitable for offline use.

---

## 📚 Usage

### Basic command

```bash
./403-Killchain -u https://target.com/admin
```

### Flags

| Flag | Description | Example |
|------|-------------|---------|
| `-u` | Target URL (required) | `-u https://example.com/admin` |
| `--only` | Run only specific engines (comma‑separated) | `--only nomore403,byp4xx` |
| `--skip` | Skip specific engines (comma‑separated) | `--skip bypass-403.sh,dontgo403` |
| `-l` | List all embedded engines and exit | `./403-Killchain -l` |
| `-h` | Show banner and help | `./403-Killchain -h` |

### Example with engine selection

```bash
# Run only the path fuzzer and the encoding engine
./403-Killchain -u https://target.com/admin --only path-fuzzer,encoding

# Run everything except the header mutator
./403-Killchain -u https://target.com/admin --skip header-mutator
```

---

## 📊 Comparison with Standalone Tools

| Capability | Standalone tools | 403‑Killchain |
|------------|:---------------:|:-------------:|
| Multiple engines | ❌ | ✅ |
| Parallel execution | ❌ | ✅ |
| Unified output | ❌ | ✅ |
| Single binary | ~ | ✅ |
| No external dependencies | ~ | ✅ |
| Full technique coverage | partial | ✅ |

---

## 🛠️ Development

### Prerequisites
- Go 1.20 or higher

### Build and Test

```bash
# Clone the repository
git clone https://github.com/Archsec-Emman/403-Killchain.git
cd 403-Killchain

# Build the binary
CGO_ENABLED=0 go build -o 403-Killchain

# Run tests (if any)
go test ./...
```

### Project Structure

```
403-Killchain/
├── main.go                 # Entry point and CLI parsing
├── go.mod                  # Go module definition
├── go.sum                  # Dependency checksums
├── LICENSE                 # MIT License
├── README.md               # This file
└── ...
```

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues, feature requests, or pull requests.

### Ways to Contribute
- **Report bugs** – Open an issue with clear reproduction steps.
- **Suggest new bypass techniques** – Propose additional payloads or headers.
- **Improve documentation** – Fix typos, add examples, or clarify usage.
- **Add engine support** – Help integrate more open‑source bypass tools.
- **Write tests** – Increase code coverage and reliability.

### Contribution Guidelines
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/amazing-feature`).
3. Commit your changes (`git commit -m 'Add some amazing feature'`).
4. Push to the branch (`git push origin feature/amazing-feature`).
5. Open a Pull Request.

Please ensure your code follows the existing style and includes appropriate tests.

### Code of Conduct
This project aims to be welcoming to all contributors. Please treat others with respect and professionalism.

---

## 📄 License

This project is licensed under the **MIT License** – see the [LICENSE](LICENSE) file for details.

The embedded engines contain code from various open‑source projects. See `THIRD_PARTY_LICENSES.md` for full attributions (to be added).

---

## 📬 Contact & Support

- **GitHub Issues**: [https://github.com/Archsec-Emman/403-Killchain/issues](https://github.com/Archsec-Emman/403-Killchain/issues)
- **Author**: [Archsec-Emman](https://github.com/Archsec-Emman)
- **Organisation**: [Arc‑Cyber‑Arsenal](https://github.com/Arc-Cyber-Arsenal)

---

*If this tool saves you time during assessments, please consider giving the repository a star.* ⭐
```
