# Caddy with ClouDNS

[![Build Caddy with ClouDNS](https://github.com/reality-solutions/caddy-with-cloudns/actions/workflows/build-caddy.yml/badge.svg)](https://github.com/reality-solutions/caddy-with-cloudns/actions/workflows/build-caddy.yml)

This repository publishes unofficial Caddy binaries that include the
[`github.com/caddy-dns/cloudns`](https://github.com/caddy-dns/cloudns) DNS
provider module.

The GitHub Actions workflow checks weekly for a new stable release of
[Caddy](https://github.com/caddyserver/caddy). When a new Caddy release is
available, it builds Caddy with the latest stable ClouDNS module using
[`xcaddy`](https://github.com/caddyserver/xcaddy), then publishes the binaries
as a GitHub release.

## Downloads

Download builds from the [Releases](https://github.com/reality-solutions/caddy-with-cloudns/releases) page.

The current build targets are:

| Operating system | Architecture | Archive | Executable in archive |
| --- | --- | --- | --- |
| Linux | AMD64 | `.tar.gz` | `caddy` |
| Linux | ARM64 | `.tar.gz` | `caddy` |
| Windows | AMD64 | `.zip` | `caddy.exe` |
| Windows | ARM64 | `.zip` | `caddy.exe` |

Each archive places the executable at its root. The executable names are
always `caddy` for Linux and `caddy.exe` for Windows. They are not derived from
the release tag.

Archives also include:

- `BUILD-METADATA.txt`, which records the source and build versions
- `GO-MODULES.txt`, which records the Go module build information
- `LICENSE-CADDY`
- `LICENSE-CLOUDNS`
- `CADDY-VERSION.txt`

macOS builds are not currently published.

## Verify a download

Download `checksums.txt` from the same release as the archive. On Linux or
another system with `sha256sum`, run:

```sh
sha256sum -c checksums.txt --ignore-missing
```

On Windows PowerShell, calculate the hash and compare it with the matching
line in `checksums.txt`:

```powershell
Get-FileHash .\caddy_2.11.4_cloudns_1.2.0_windows_amd64.zip -Algorithm SHA256
```

Replace the example version numbers with the versions in the release you
downloaded.

## Install

### Linux

Extract the archive and install the executable. For example:

```sh
tar -xzf caddy_2.11.4_cloudns_1.2.0_linux_amd64.tar.gz
sudo install -m 0755 caddy /usr/local/bin/caddy
caddy version
```

To verify that the DNS provider is included:

```sh
caddy list-modules | grep dns.providers.cloudns
```

### Windows

Extract the ZIP archive. For example, in PowerShell:

```powershell
Expand-Archive .\caddy_2.11.4_cloudns_1.2.0_windows_amd64.zip -DestinationPath .\caddy
.\caddy\caddy.exe version
```

Add the extracted directory to `PATH` if you want to invoke `caddy.exe` from
any directory.

## Configure ClouDNS

The module provides the Caddy module `dns.providers.cloudns`. Configure the
ClouDNS API credentials in the environment of the Caddy process:

- `CLOUDNS_AUTH_ID`, or `CLOUDNS_SUB_AUTH_ID`
- `CLOUDNS_AUTH_PASSWORD`

Then use the DNS challenge in a Caddyfile:

```Caddyfile
example.com {
    tls {
        dns cloudns
    }
}
```

The build workflow does not use ClouDNS credentials and never performs DNS
operations. Credentials are only needed when Caddy runs and provisions a
certificate.

See the [ClouDNS module documentation](https://github.com/caddy-dns/cloudns)
for API restrictions, explicit credential configuration, and retry settings.

## Build locally

Install Go and `xcaddy`, then build with an explicit output name:

```sh
go install github.com/caddyserver/xcaddy/cmd/xcaddy@v0.4.7
xcaddy build v2.11.4 \
    --with github.com/caddy-dns/cloudns@v1.2.0 \
    --output caddy
```

For a Windows build, set the target and output name explicitly:

```sh
GOOS=windows GOARCH=amd64 xcaddy build v2.11.4 \
    --with github.com/caddy-dns/cloudns@v1.2.0 \
    --output caddy.exe
```

The workflow uses the latest stable ClouDNS release when it builds a new Caddy
release. It does not create a new build only because ClouDNS released a new
version; that version is picked up by the next Caddy build.

## Release tags

Release tags use this format:

```text
caddy-v2.11.4-cloudns
```

The release notes identify the exact ClouDNS module version used in that
build. A release is created only after all four platform archives have built,
passed validation, and uploaded successfully.

## License and disclaimer

Caddy is distributed under the [Apache License 2.0](https://github.com/caddyserver/caddy/blob/master/LICENSE).
The ClouDNS Caddy module is distributed under the [MIT License](https://github.com/caddy-dns/cloudns/blob/main/LICENSE).
`xcaddy` is used as a build tool and is distributed under the [Apache License 2.0](https://github.com/caddyserver/xcaddy/blob/master/LICENSE).
Relevant license files are included in each binary archive.

These are unofficial custom builds. This repository is not affiliated with,
endorsed by, or sponsored by the Caddy or ClouDNS projects. Caddy and ClouDNS
names and marks remain the property of their respective owners.
