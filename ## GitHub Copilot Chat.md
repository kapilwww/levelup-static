## GitHub Copilot Chat

- Extension: 0.50.0 (prod)
- VS Code: 1.122.0 (6a49527b96e326fe62fbdb56f60e16877c9aa724)
- OS: linux 6.19.14+kali-amd64 x64
- GitHub Account: kapilwww

## Network

User Settings:
```json
  "http.systemCertificatesNode": true,
  "github.copilot.advanced.debug.useElectronFetcher": true,
  "github.copilot.advanced.debug.useNodeFetcher": false,
  "github.copilot.advanced.debug.useNodeFetchFetcher": true
```

Connecting to https://api.github.com:
- DNS ipv4 Lookup: 20.205.243.168 (4 ms)
- DNS ipv6 Lookup: Error (18 ms): getaddrinfo ENOTFOUND api.github.com
- Proxy URL: None (2 ms)
- Electron fetch (configured): HTTP 504 (5334 ms)
- Node.js https: HTTP 504 (341 ms)
- Node.js fetch: HTTP 504 (122 ms)

Connecting to https://api.githubcopilot.com/_ping:
- DNS ipv4 Lookup: 140.82.113.21 (34 ms)
- DNS ipv6 Lookup: Error (11 ms): getaddrinfo ENOTFOUND api.githubcopilot.com
- Proxy URL: None (25 ms)
- Electron fetch (configured): HTTP 200 (829 ms)
- Node.js https: HTTP 200 (923 ms)
- Node.js fetch: HTTP 200 (987 ms)

Connecting to https://copilot-proxy.githubusercontent.com/_ping:
- DNS ipv4 Lookup: 138.91.182.224 (101 ms)
- DNS ipv6 Lookup: Error (119 ms): getaddrinfo ENOTFOUND copilot-proxy.githubusercontent.com
- Proxy URL: None (10 ms)
- Electron fetch (configured): HTTP 200 (853 ms)
- Node.js https: 