# skill-audit

A security scanner for OpenClaw skills. Scans skill folders and `.skill` files for prompt injection, data exfiltration, malicious code, and more — before you install them.

Built at the [OpenClaw Hackathon Munich](https://baiosphere.org/events/2026/openclaw-hackathon-muenchen-24-03) @ jambit.

## Install from ClawHub

```
clawhub install openclaw-skill-audit
```

## Usage

Scan a skill folder:

```bash
python3 scan_skill.py path/to/skill
```

JSON output:

```bash
python3 scan_skill.py path/to/skill --json
```

Scan a packaged `.skill` file:

```bash
python3 scan_skill.py some-skill.skill
```

Bulk scan all installed skills:

```bash
for d in ~/.openclaw/skills/*/; do
  python3 scan_skill.py "$d"
done
```

## What it detects

1. **Prompt injection** — hidden instructions, identity overrides, invisible unicode, HTML comments
2. **Data exfiltration** — base64+POST, reverse shells, data capture services
3. **Dangerous code** — eval/exec, dynamic imports, unsafe deserialization, subprocess
4. **File system abuse** — path traversal, SSH key access, system file reads
5. **Network connections** — URL extraction, hardcoded IPs, known API endpoints
6. **Secret access** — env var reads, API key patterns, credential references
7. **Permission scope** — required binaries, env vars, network-capable tools

## Risk levels

- 🟢 **Low** — no concern
- 🟡 **Medium** — review recommended
- 🔴 **High** — likely dangerous
- ⛔ **Critical** — almost certainly malicious

## Pre-install hook

skill-audit works as a pre-install hook for OpenClaw. When a skill is installed via `clawhub install`, skill-audit scans it automatically before activation. See [SKILL.md](SKILL.md) for the hook workflow.

## Limitations

Static analysis catches patterns, not intent. It cannot detect:

- Logic-level attacks (subtly biased outputs)
- Obfuscated code beyond known patterns
- Runtime-only behaviour (code fetched from a URL then executed)

Combine with manual review for high-stakes deployments.

## Tech

- 712 lines of Python
- Zero external dependencies
- AST parsing for Python, regex patterns for Bash/JS
- Works on skill folders and `.skill` archives (zip)

## Authors

Built by Flo & Neo at the OpenClaw Hackathon Munich, 24 March 2026.

## Licence

MIT
