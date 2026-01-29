# Universal Log Parser (ULP)

A Python library and CLI tool to automatically detect, parse, and normalize logs from any format.

## Features

- **Auto-Detection**: Automatically identifies log formats without configuration
- **Multiple Formats**: Supports JSON, Apache, Nginx, Syslog (RFC 3164 & 5424), Python logging
- **Normalized Output**: All logs converted to a common schema
- **CLI Tool**: Parse and detect formats from the command line
- **Extensible**: Easy to add custom parsers

## Installation

```bash
pip install ulp
```

## Quick Start

### CLI Usage

```bash
# Parse a log file (format auto-detected)
ulp parse /var/log/nginx/access.log

# Detect format only
ulp detect /var/log/syslog

# Parse with specific format
ulp parse --format json_structured app.log

# Filter by level and output as JSON
ulp parse --level error --output json app.log
```

### Library Usage

```python
from ulp import parse, detect_format, LogLevel

# Detect format
format_name, confidence = detect_format("access.log")
print(f"Format: {format_name} ({confidence:.0%})")

# Parse logs
entries = parse("access.log")

# Filter errors
errors = [e for e in entries if e.level >= LogLevel.ERROR]

# Access normalized fields
for entry in entries:
    print(f"{entry.timestamp} [{entry.level.name}] {entry.message}")
```

## Supported Formats

| Format | Parser Name | Description |
|--------|-------------|-------------|
| JSON/JSONL | `json` | JSON structured logs |
| Apache Combined | `apache_combined` | Apache Combined Log Format |
| Apache Common | `apache_common` | Apache Common Log Format |
| Nginx Access | `nginx_access` | Nginx default access log |
| Nginx Error | `nginx_error` | Nginx error log |
| Syslog RFC 3164 | `syslog_rfc3164` | BSD syslog format |
| Syslog RFC 5424 | `syslog_rfc5424` | Modern syslog format |
| Python Logging | `python_logging` | Python standard logging |
| Generic | `generic` | Fallback for unknown formats |

## License

MIT License
