<p align="center">
  <img src="https://sncfoundation.github.io/logos/sheetmesh.svg" width="96" alt="Sheetmesh logo">
</p>

<h1 align="center">Sheetmesh</h1>

<p align="center"><b>Federate spreadsheets into one service mesh</b><br>
A <a href="https://sncfoundation.github.io">Sheet-Native Computing Foundation</a> project &#183; analog of <b>a service mesh</b></p>

---

**Status:** 📝 Unsaved Draft &#183; this project is a proposal. Design notes and contributions are welcome.

## About

Federate spreadsheets into one service mesh Part of the spreadsheet-native stack — the Sheet stays the source of truth, and it reconciles.

## Usage

`sheetmesh` is a small bash CLI that federates several Sheeternetes clusters —
each one a Google Sheet fronted by an Apps Script apiserver — into a single
mesh. It gives you cross-sheet service discovery: register your workbooks as
peers, then ask which cluster(s) host a given Deployment.

Requires `bash`, `curl`, and `jq`. Peers are stored in
`${SHEETMESH_HOME:-~/.sheetmesh}/peers.json`.

```bash
# Register peer clusters (name, Apps Script /exec URL, token)
sheetmesh peer add prod https://script.google.com/macros/s/AAAA/exec s3cr3t
sheetmesh peer add edge https://script.google.com/macros/s/BBBB/exec h0tdog

# List / remove peers
sheetmesh peer list
sheetmesh peer remove edge

# Cross-sheet service discovery: which cluster(s) host a Deployment?
sheetmesh resolve web
# SERVICE  CLUSTER  REPLICAS  IMAGE
# web      prod     3         nginx:latest
# web      edge     1         caddy

# List every service across the mesh with its home cluster
sheetmesh services
```

### Smoke-testing without a live apiserver

Pass `--mock` to feed every peer a canned deployments response, or export a
`SHEET_JSON_<PEER>` variable (peer name upper-cased, non-alphanumerics `->` `_`)
to stub a single peer:

```bash
sheetmesh resolve web --mock

export SHEET_JSON_PROD='{"items":[{"name":"web","image":"nginx","replicas":3}]}'
sheetmesh resolve web
```

## Get involved

- 📋 Tracking issue &amp; design: [sncfoundation/sheeternetes#6](https://github.com/sncfoundation/sheeternetes/issues/6)
- 🗺️ [SNCF Landscape](https://sncfoundation.github.io/landscape.html)
- 🧩 [All projects](https://sncfoundation.github.io/projects.html)
- ⚖️ [Governance &amp; how to contribute](https://github.com/sncfoundation/governance)
- 🎓 [Get certified (CSFE)](https://sncfoundation.github.io/certification.html)

## Status legend

Everything starts as an **Unsaved Draft**. It reconciles up the tiers from there — see the
[maturity model](https://sncfoundation.github.io/foundation.html#maturity).

---

<sub>Licensed under Apache-2.0. The SNCF does not recommend running production on a spreadsheet. If you do, please film it.</sub>
