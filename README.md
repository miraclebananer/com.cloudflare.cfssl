# CFSSL Flatpak

Cloudflare's PKI/TLS toolkit packaged as a Flatpak.

## Installation

```bash
flatpak install com.cloudflare.cfssl.flatpakref
```

Or build from source:
```bash
flatpak-builder --user --install build-dir com.cloudflare.cfssl.yaml
```

## Usage

### Method 1: Direct flatpak run (works immediately)

```bash
# Main cfssl command
flatpak run com.cloudflare.cfssl version

# Other commands require --command flag
flatpak run --command=cfssljson com.cloudflare.cfssl --help
flatpak run --command=cfssl-certinfo com.cloudflare.cfssl --help
flatpak run --command=cfssl-scan com.cloudflare.cfssl --help
```

### Method 2: Add to PATH (recommended for frequent use)

Add the flatpak exports directory to your PATH once:

```bash
echo 'export PATH="$HOME/.local/share/flatpak/exports/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Then use the main command with its full name:
```bash
com.cloudflare.cfssl version
```

For other commands, still use flatpak run with --command:
```bash
flatpak run --command=cfssljson com.cloudflare.cfssl --help
```

### Method 3: Create convenient aliases (optional)

For even easier access, add aliases to `~/.bash_aliases` or `~/.bashrc`:

```bash
alias cfssl='flatpak run com.cloudflare.cfssl'
alias cfssljson='flatpak run --command=cfssljson com.cloudflare.cfssl'
alias cfssl-bundle='flatpak run --command=cfssl-bundle com.cloudflare.cfssl'
alias cfssl-certinfo='flatpak run --command=cfssl-certinfo com.cloudflare.cfssl'
alias cfssl-newkey='flatpak run --command=cfssl-newkey com.cloudflare.cfssl'
alias cfssl-scan='flatpak run --command=cfssl-scan com.cloudflare.cfssl'
alias mkbundle='flatpak run --command=mkbundle com.cloudflare.cfssl'
alias multirootca='flatpak run --command=multirootca com.cloudflare.cfssl'
```

Then source your shell configuration:
```bash
source ~/.bashrc
```

Now you can use commands directly:
```bash
cfssl version
cfssljson --help
cfssl-certinfo --help
```

## Available Commands

- **cfssl** - Main certificate operations tool
- **cfssljson** - JSON output processor
- **mkbundle** - Certificate pool bundler
- **multirootca** - Multi-root CA server
- **cfssl-bundle** - Certificate bundler
- **cfssl-certinfo** - Certificate info extractor
- **cfssl-newkey** - Key/CSR generator
- **cfssl-scan** - Security scanner

## Example: Generate a Certificate

```bash
# Create a config file
cat > csr.json <<EOF
{
  "CN": "example.com",
  "key": {
    "algo": "rsa",
    "size": 2048
  }
}
EOF

# Generate key and CSR
flatpak run com.cloudflare.cfssl genkey csr.json | \
  flatpak run --command=cfssljson com.cloudflare.cfssl -bare server

# Files created: server-key.pem, server.csr
```

## License

CFSSL is licensed under the BSD-2-Clause license.
