# Container Security Hardening

Praxisprojekt (PP6): Systematisches Härten eines Docker-Containers nach dem **CIS Docker Benchmark v1.6**.

## Ergebnisse

| Metrik | Vorher | Nachher | Delta |
|--------|--------|---------|-------|
| CIS Score (Section 4+5) | 54,1 % | 97,4 % | +43,3 pp |
| Trivy CVEs | 143 | 0 | −143 |

## Maßnahmen

- **Base-Image**: `python:3.7-slim-buster` → Chainguard (distroless, nonroot, pinned digest)
- **Multi-Stage Build**: Builder für pip, Runtime ohne pip/shell
- **Supply Chain**: `--require-hashes` mit pip-compile SHA256-Hashes
- **Runtime Hardening**: read_only FS, cap_drop ALL, no-new-privileges, Ressourcenlimits
- **CI/CD**: GitHub Actions + Trivy Scanner (Push, PR, wöchentlich)

## Struktur

```
app/           → Flask-App + gelockte Dependencies
docker/        → Dockerfile (Multi-Stage) + docker-compose.yml
scans/         → Vorher/Nachher CIS-Benchmark + Trivy-Reports
.github/       → Trivy CI Pipeline
```

## Tech-Stack

Python · Flask · Chainguard · Trivy · Docker · GitHub Actions