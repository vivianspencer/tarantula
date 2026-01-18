# Project Context

## Purpose
**Tarantula** is an open-source desktop web crawler and SEO auditing tool, designed as a fast, lightweight alternative to Screaming Frog. Built in Go for maximum performance, Tarantula enables comprehensive technical SEO audits by crawling websites and analysing key on-page elements, site structure, and technical issues.

**Goal:** Provide SEO professionals and developers with a free, performant tool to audit websites, identify technical issues, and export actionable data for optimisation.

**License:** AGPLv3

## Tech Stack

### Core Technologies
- **Go 1.23+** - Primary language for performance and concurrency
- **Colly v2** - Web crawling framework
- **GoLand** - Primary IDE
- **SQLite** - Local data storage for crawl results
- **Excelize v2** - XLSX export generation

### Desktop Framework (TBD)
- **Wails v2** - Go-native desktop framework (preferred for keeping everything in Go)
- **Alternative:** Tauri (Rust-based, smaller binaries)

### Future Integrations
- **Google Analytics API** - Traffic and engagement data
- **Google Search Console API** - Search performance metrics
- **PageSpeed Insights API** - Performance scoring
- **DataForSEO API** - SERP data, backlink analysis, keyword research, and competitor insights
- **Chromedp** - JavaScript rendering for SPAs

## Project Conventions

### Code Style
- **Standard Go formatting** - Use `gofmt` and `goimports`
- **British English** - All comments, docs, and UI copy in British English
- **Error handling** - Explicit error returns, no panics in production code
- **Package naming** - Short, lowercase, single-word package names
- **Constants** - UPPER_SNAKE_CASE for exported constants
- **Variables/Functions** - camelCase for private, PascalCase for exported
- **Comments** - Full sentences with proper punctuation, explain *why* not *what*

```go
// Good
// AnalyseMetaDescription checks description length and provides SEO recommendations.
func AnalyseMetaDescription(desc string) Recommendation {}

// Bad
// check meta desc
func checkMetaDesc(d string) {}
```

### Architecture Patterns

**Layered Architecture:**
```
├── cmd/           # Application entry points
├── internal/      # Private application code
│   ├── crawler/   # Core crawling logic
│   ├── analyser/  # SEO analysis rules
│   ├── storage/   # Database interactions
│   ├── exporter/  # CSV/XLSX/Sheets export
│   └── ui/        # Desktop UI components
├── pkg/           # Public reusable packages
└── config/        # Configuration files
```

**Key Patterns:**
- **Repository Pattern** - Abstract database access
- **Strategy Pattern** - Pluggable analysers for different SEO rules
- **Observer Pattern** - Progress updates during crawling
- **Dependency Injection** - Pass dependencies explicitly, no globals

### Testing Strategy

**Test Coverage Goals:**
- **Unit tests:** 80%+ coverage for business logic
- **Integration tests:** Critical paths (crawling, exporting)
- **Table-driven tests:** For analysers with multiple scenarios

**Testing Standards:**
```go
// Use subtests for multiple scenarios
func TestAnalyseTitle(t *testing.T) {
    tests := []struct {
        name     string
        title    string
        expected SeverityLevel
    }{
        {"optimal length", "Great Title Here", SeverityLow},
        {"too long", strings.Repeat("a", 100), SeverityHigh},
    }
    
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            // test logic
        })
    }
}
```

**Test Naming:**
- `TestFunctionName` for unit tests
- `TestFunctionName_Integration` for integration tests
- Benchmark tests: `BenchmarkFunctionName`

### Git Workflow

**Branching Strategy:**
- `main` - Production-ready code
- `develop` - Integration branch for features
- `feature/<name>` - New features
- `bugfix/<name>` - Bug fixes
- `release/<version>` - Release preparation

**Commit Conventions (Conventional Commits):**
```
feat: add robots.txt parser
fix: correct redirect chain detection
docs: update installation instructions
refactor: simplify meta description analyser
test: add tests for sitemap generation
perf: optimise concurrent crawling
```

**Pull Request Requirements:**
- All tests pass
- No linting errors
- Updated documentation if API changes
- Descriptive PR title and description

## Domain Context

### SEO Terminology
- **Crawl depth** - Number of clicks from homepage
- **Index coverage** - Pages eligible for search engine indexing
- **Canonical URL** - Preferred version of duplicate pages
- **Meta robots** - Directives controlling indexing (`noindex`, `nofollow`)
- **hreflang** - Language and regional targeting attributes
- **Redirect chains** - Multiple sequential redirects (bad for SEO)
- **Orphan pages** - Pages with no internal links
- **Crawl budget** - Resources search engines allocate to crawling a site

### HTTP Status Codes (SEO Context)
- **2xx** - Success (crawlable)
- **3xx** - Redirects (301 permanent, 302 temporary, 307/308 modern variants)
- **4xx** - Client errors (404 not found, 410 gone)
- **5xx** - Server errors (should be fixed immediately)

### Key SEO Rules
- **Title length:** 50-60 characters optimal
- **Meta description:** 150-160 characters optimal
- **H1 tags:** One per page (multiple H1s acceptable if semantic HTML5)
- **Image alt text:** Required for accessibility and SEO
- **URL structure:** Short, descriptive, lowercase, hyphens not underscores
- **Canonical tags:** Required for duplicate content management

### DataForSEO Integration Context
- **SERP data** - Search Engine Results Page rankings, featured snippets, and competitor positions
- **Backlink analysis** - Inbound link profiles, anchor text distribution, referring domains
- **Keyword research** - Search volumes, difficulty scores, related keywords
- **Domain analytics** - Domain authority metrics, organic traffic estimates, ranking distribution

## Important Constraints

### Technical Constraints
- **Politeness:** Respect `robots.txt`, implement rate limiting (default 50-100ms delay)
- **Memory management:** Support crawling sites with 100k+ pages without excessive memory
- **Cross-platform:** Must run on macOS, Windows, Linux
- **No external runtime dependencies:** Single binary distribution

### Performance Targets (Goals)
- **Speed:** 500+ pages/second on modern hardware (target goal)
- **Concurrency:** Support 50-100+ concurrent requests
- **Startup time:** <200ms cold start
- **Binary size:** <20MB (without UI assets)

### Legal/Ethical Constraints
- Respect `robots.txt` directives by default
- Option to override (with warnings) for authorised audits
- Rate limiting to avoid DDoS-ing target sites
- User-agent string identifies as "Tarantula/x.x.x"

### Data Privacy
- All data stored locally, no cloud sync
- No telemetry or usage tracking
- Option to exclude sensitive patterns from exports

## External Dependencies

### Core Libraries
- **github.com/gocolly/colly/v2** - Web crawling framework
- **github.com/mattn/go-sqlite3** - SQLite database driver
- **github.com/xuri/excelize/v2** - Excel file generation
- **github.com/temoto/robotstxt** - robots.txt parser

### Future APIs
- **Google Analytics API v4** - Requires OAuth2 authentication
- **Google Search Console API** - Requires OAuth2 authentication, limited to verified properties
- **PageSpeed Insights API** - Requires API key, rate limited
- **DataForSEO API** - Requires API credentials, provides SERP data, backlinks, keywords, and domain analytics
- **Google Sheets API** - Direct export to user's Google Sheets

### Development Tools
- **github.com/golangci/golangci-lint** - Comprehensive linting
- **github.com/stretchr/testify** - Testing assertions and mocks
- **github.com/wailsapp/wails** - Desktop framework (v2)

### Optional Dependencies
- **github.com/chromedp/chromedp** - Headless Chrome for JS rendering (future feature)
- **github.com/PuerkitoBio/goquery** - HTML parsing (if more DOM manipulation needed)

---

## Getting Started

### Prerequisites
```bash
go version  # 1.23 or higher
```

### Installation
```bash
git clone https://github.com/vivianspencer/tarantula.git
cd tarantula
go mod download
go run cmd/tarantula/main.go  # Once implemented
```

### Quick Start Example
```bash
# Crawl a website
tarantula crawl https://example.com --depth 5

# Export results
tarantula export --format xlsx --output report.xlsx
```

### Project Structure
```
tarantula/
├── cmd/
│   └── tarantula/
│       └── main.go          # Application entry point
├── internal/
│   ├── crawler/             # Crawling engine
│   ├── analyser/            # SEO analysis logic
│   │   ├── metadata.go      # Title, description analysis
│   │   ├── links.go         # Link analysis, broken links
│   │   ├── redirects.go     # Redirect chains, loops
│   │   └── hreflang.go      # hreflang validation
│   ├── storage/             # SQLite persistence
│   ├── exporter/            # Export functionality
│   └── ui/                  # Desktop UI
├── pkg/
│   └── models/              # Shared data structures
├── config/
│   └── config.yaml          # Default configuration
├── docs/                    # Documentation
├── go.mod
└── README.md
```

---

## Roadmap

### v1.0 - Core Features (MVP)
- [ ] Basic crawling with Colly
- [ ] CSV/XLSX export
- [ ] Broken link detection
- [ ] Page title and meta data analysis
- [ ] Meta robots audit
- [ ] hreflang validation
- [ ] Duplicate page detection
- [ ] XML sitemap review
- [ ] Redirect audit (chains, loops)
- [ ] Basic desktop UI

### v1.1 - Enhanced Features
- [ ] Google Sheets export
- [ ] Robots.txt integration
- [ ] Advanced filtering and search
- [ ] Custom crawl configurations
- [ ] Report templates

### v2.0 - Advanced Features
- [ ] XML sitemap generation
- [ ] Google Analytics integration
- [ ] Search Console integration
- [ ] PageSpeed Insights integration
- [ ] DataForSEO API integration (SERP data, backlinks, keywords)
- [ ] JSON-LD schema validation
- [ ] JavaScript rendering with Chromedp

---

## Contributing

Contributions are welcome! See [CONTRIBUTING.md](../CONTRIBUTING.md) for guidelines on:
- Reporting bugs and issues
- Suggesting new features
- Submitting pull requests
- Code review process

Please ensure all contributions follow the project conventions outlined above and include appropriate tests.