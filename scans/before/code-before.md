# Code Before Security Baseline

## Environment
### Docker Benchmark
* Repo auf einem Proxmox LXC gecloned
* debian-12-standard_12.12-1_amd64
* Docker Engine 29.5.3 installiert
* Image gebaut und Docker Benchmark Repo gecloned 
* Ergebnisse in auto-cis-benchmark.md

### Trivy
* Github Action via workflow security-scan.yml
* aquasecurity/trivy-action@v0.36.0
* Ohne Failgate > Kommt erst nach Security Baseline
* Gespeichert als JSON und txt File


## Python
```python
import os
import platform
import sys
from flask import Flask, jsonify

app = Flask(__name__)


@app.route("/")
def index():
    return jsonify({"status": "ok", "service": "pp6-baseline"})


@app.route("/health")
def health():
    return jsonify({"status": "healthy"}), 200


@app.route("/info")
def info():
    return jsonify({
        "python_version": sys.version,
        "platform": platform.platform(),
        "user": os.getenv("USER", "unknown"),
        "uid": os.getuid(),
        "gid": os.getgid(),
        "hostname": platform.node(),
        "env_vars_count": len(os.environ),
    })


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=False)
```
## Dockerfile
```Dockerfile
FROM python:3.7-slim-buster

WORKDIR /app

COPY app/requirements.txt .
RUN pip install -r requirements.txt

COPY app/ .

EXPOSE 5000

CMD ["python", "app.py"]
```

## requirements.txt
```
Flask==2.0.3
Werkzeug==2.0.3
```