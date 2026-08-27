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

Each archive places the executable at its root, named `caddy` or `caddy.exe`
(not the versioned archive name).

Archives also include:

- `BUILD-METADATA.txt`, which records the source and build versions
- `GO-MODULES.txt`, which records the Go module build information
- `LICENSE-CADDY`
- `LICENSE-CLOUDNS`
- `CADDY-VERSION.txt`

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

## Documentation

- [Caddy documentation](https://caddyserver.com/docs/)
- [ClouDNS module documentation](https://github.com/caddy-dns/cloudns)

## Release tags

Release tags include both the Caddy and ClouDNS module versions:

```text
caddy-v2.11.4-cloudns-v1.2.0
```

A release is created only after all four platform archives have built, passed
validation, and uploaded successfully.

## License and disclaimer

Caddy is distributed under the [Apache License 2.0](https://github.com/caddyserver/caddy/blob/master/LICENSE).
The ClouDNS Caddy module is distributed under the [MIT License](https://github.com/caddy-dns/cloudns/blob/main/LICENSE).
`xcaddy` is used as a build tool and is distributed under the [Apache License 2.0](https://github.com/caddyserver/xcaddy/blob/master/LICENSE).
Relevant license files are included in each binary archive.

These are unofficial custom builds. This repository is not affiliated with,
endorsed by, or sponsored by the Caddy or ClouDNS projects. Caddy and ClouDNS
names and marks remain the property of their respective owners.
