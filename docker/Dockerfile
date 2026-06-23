# ============================================================
# Stage 1: BUILDER — installiert Python-Deps mit Hash-Check
# ============================================================
FROM cgr.dev/chainguard/python:latest-dev@sha256:a62f72f9480c548cd3d7d07426a490aab2a38c4fa89b9665dc36780ed85aca9f AS builder

USER root
WORKDIR /app

# requirements.txt (mit Hashes) reinkopieren
COPY app/requirements.txt .

# pip install mit Hash-Verifikation in ein eigenes Verzeichnis
# --target=/install → Pakete liegen isoliert, lassen sich easy kopieren
RUN pip install \
    --require-hashes \
    --target=/install \
    -r requirements.txt


# ============================================================
# Stage 2: RUNTIME — minimal, nonroot (uid 65532), keine shell, kein pip
# ============================================================
FROM cgr.dev/chainguard/python:latest@sha256:7036bf1b4a132cccad9f68ea9b9945642df49ea7f940194fbcb0429db8da0e48

WORKDIR /app

# Deps aus Build-Stage übernehmen
COPY --from=builder /install /install

# App-Code rein
COPY app/ /app/

# Python findet die Deps unter /install
ENV PYTHONPATH=/install

EXPOSE 5000

# HEALTHCHECK ohne shell → exec form mit python -c
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD ["python", "-c", "import urllib.request,sys; sys.exit(0 if urllib.request.urlopen('http://localhost:5000/health').status==200 else 1)"]

# chainguard/python hat python als ENTRYPOINT → CMD enthält nur die Args
CMD ["app.py"]