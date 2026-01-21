# Tarantula

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL_v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![Go Version](https://img.shields.io/badge/Go-1.23+-00ADD8?logo=go)](https://go.dev/)

**Tarantula** is an open-source desktop web crawler and SEO auditing tool, designed as a fast, lightweight alternative. Built in Go for maximum performance, Tarantula enables comprehensive technical SEO audits by crawling websites and analysing key on-page elements, site structure, and technical issues.

## 🎯 Purpose

Provide SEO professionals and developers with a **free, performant tool** to audit websites, identify technical issues, and export actionable data for optimisation.

## ✨ Features

### Current Status: In Development

The project is currently in active development. The foundational web crawler is being implemented.

### Planned v1.0 Features (MVP)

- ✅ **Basic web crawling** with Colly framework
  - Configurable depth and concurrency
  - JavaScript rendering support (Chromedp)
  - Robots.txt compliance with optional bypass
  - Real-time progress reporting
- ✅ **SQLite data storage** for crawl results
- ✅ **CLI interface** for crawl configuration
- 🔲 CSV/XLSX export
- 🔲 Broken link detection
- 🔲 Page title and meta data analysis
- 🔲 Meta robots audit
- 🔲 hreflang validation
- 🔲 Duplicate page detection
- 🔲 XML sitemap review
- 🔲 Redirect audit (chains, loops)
- 🔲 Basic desktop UI

### Future Enhancements (v1.1+)

- Google Sheets export
- Advanced filtering and search
- Custom crawl configurations
- Report templates
- XML sitemap generation
- Google Analytics integration
- Search Console integration
- PageSpeed Insights integration
- DataForSEO API integration (SERP data, backlinks, keywords)
- JSON-LD schema validation

## 🚀 Getting Started

### Prerequisites

- **Go 1.23 or higher** - [Download Go](https://go.dev/dl/)
- **CGO enabled** (required for SQLite)
- **Chromium** (auto-downloaded on first `--render-js` usage)

Check your Go version:
```bash
go version
```

### Installation

#### From Source

```bash
# Clone the repository
git clone https://github.com/vivianspencer/tarantula.git
cd tarantula

# Download dependencies
go mod download

# Build the binary
go build -o tarantula cmd/tarantula/main.go

# Run Tarantula
./tarantula --version
```

#### Pre-built Binaries

Pre-built binaries for macOS, Windows, and Linux will be available once v1.0 is released.

## 📖 Usage

### Basic Crawl

Crawl a website from a seed URL:

```bash
tarantula crawl https://example.com
```

### Crawl with Depth Limit

Limit crawl to 3 levels deep from the seed URL:

```bash
tarantula crawl https://example.com --depth 3
```

### High-Speed Crawl

Increase concurrency for faster crawling:

```bash
tarantula crawl https://example.com --concurrency 10 --delay 100
```

### JavaScript-Heavy Sites

Enable JavaScript rendering for single-page applications (SPAs):

```bash
tarantula crawl https://example.com --render-js
```

### Ignore Robots.txt (Authorised Audits Only)

Bypass robots.txt for sites you own or have permission to audit:

```bash
tarantula crawl https://example.com --ignore-robots
```

⚠️ **Warning**: Only use `--ignore-robots` for authorised audits. Respect site owners' crawling preferences.

### Custom Database Location

Specify a custom database file path:

```bash
tarantula crawl https://example.com --database /path/to/crawl.db
```

### Limit Total Pages

Stop crawl after reaching maximum page count:

```bash
tarantula crawl https://example.com --max-pages 500
```

### Available Flags

| Flag | Description | Default |
|------|-------------|---------|
| `--depth <n>` | Maximum crawl depth from seed URL | unlimited |
| `--concurrency <n>` | Number of concurrent requests | 5 |
| `--delay <ms>` | Delay between requests (milliseconds) | 200 |
| `--max-pages <n>` | Maximum pages to crawl | unlimited |
| `--render-js` | Enable JavaScript rendering (Chromedp) | false |
| `--ignore-robots` | Bypass robots.txt restrictions | false |
| `--user-agent <string>` | Custom User-Agent header | `Tarantula/1.0.0` |
| `--database <path>` | SQLite database file path | `./tarantula.db` |
| `--verbose, -v` | Log each URL as crawled | false |
| `--quiet` | Suppress progress output | false |

### Get Help

Display all available commands and flags:

```bash
tarantula --help
```

## 🏗️ Architecture

Tarantula follows a layered architecture pattern:

```
├── cmd/           # Application entry points
├── internal/      # Private application code
│   ├── crawler/   # Core crawling logic (Colly integration)
│   ├── analyser/  # SEO analysis rules
│   ├── storage/   # Database interactions (SQLite)
│   ├── exporter/  # CSV/XLSX/Sheets export
│   ├── progress/  # Progress reporting (Observer pattern)
│   └── ui/        # Desktop UI components
├── pkg/           # Public reusable packages
└── config/        # Configuration files
```

**Key Patterns:**
- **Repository Pattern** - Abstract database access
- **Observer Pattern** - Progress updates during crawling
- **Strategy Pattern** - Pluggable analysers for different SEO rules
- **Dependency Injection** - Pass dependencies explicitly, no globals

## 🧪 Testing

Run all tests:

```bash
go test ./...
```

Run tests with coverage:

```bash
go test -cover ./...
```

Run benchmarks:

```bash
go test -bench=. ./...
```

## 🛠️ Development

### Code Style

- **Standard Go formatting** - Use `gofmt` and `goimports`
- **British English** - All comments, docs, and UI copy
- **Explicit error handling** - No panics in production code
- **Comments** - Explain *why*, not *what*

### Linting

Run the linter before committing:

```bash
golangci-lint run
```

### Git Workflow

**Branching:**
- `main` - Production-ready code
- `develop` - Integration branch for features
- `feature/<name>` - New features
- `bugfix/<name>` - Bug fixes

**Commit Conventions (Conventional Commits):**
```
feat: add robots.txt parser
fix: correct redirect chain detection
docs: update installation instructions
refactor: simplify meta description analyser
test: add tests for sitemap generation
perf: optimise concurrent crawling
```

## 📊 Performance Targets

- **Speed:** 500+ pages/second on modern hardware (target goal)
- **Concurrency:** Support 50-100+ concurrent requests
- **Startup time:** <200ms cold start
- **Binary size:** <20MB (without UI assets)
- **Memory:** Support crawling sites with 100k+ pages efficiently

## 🌍 Ethical Crawling

Tarantula is designed with ethical crawling practices in mind:

- **Respects robots.txt** by default
- **Rate limiting** to avoid overwhelming servers (default: 5 req/sec)
- **User-agent identification** for transparency
- **Override warnings** when bypassing restrictions

**User Responsibility:** Only crawl sites you own or have explicit permission to audit. Tarantula provides tools for authorised technical SEO work.

## 📝 License

This project is licensed under the **AGPLv3 License** - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on:
- Reporting bugs and issues
- Suggesting new features
- Submitting pull requests
- Code review process

## 🗺️ Roadmap

### v1.0 - Core Features (In Progress)
- [x] Web crawler with Colly
- [x] SQLite storage
- [x] CLI interface
- [x] Progress reporting
- [ ] CSV/XLSX export
- [ ] SEO analysis features
- [ ] Desktop UI

### v1.1 - Enhanced Features
- [ ] Google Sheets export
- [ ] Robots.txt integration
- [ ] Advanced filtering
- [ ] Custom configurations

### v2.0 - Advanced Features
- [ ] Google Analytics integration
- [ ] Search Console integration
- [ ] DataForSEO API integration
- [ ] JSON-LD schema validation

## 📧 Support

- **Issues:** [GitHub Issues](https://github.com/vivianspencer/tarantula/issues)
- **Discussions:** [GitHub Discussions](https://github.com/vivianspencer/tarantula/discussions)

## 🙏 Acknowledgements

Built with these excellent open-source libraries:
- [Colly](https://github.com/gocolly/colly) - Web crawling framework
- [Chromedp](https://github.com/chromedp/chromedp) - JavaScript rendering
- [go-sqlite3](https://github.com/mattn/go-sqlite3) - SQLite driver
- [Excelize](https://github.com/xuri/excelize) - Excel file generation

---

**Made with ❤️ for the SEO community**
