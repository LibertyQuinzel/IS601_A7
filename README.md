# QR Code Generator

Small Python project that generates QR code images from a provided URL and (optionally) builds/pushes a Docker image.

## What it is

- `main.py` is a tiny CLI that creates QR codes and writes them to the `qr_codes/` directory.
- A GitHub Actions workflow is present at `.github/workflows/ci.yml` to run a smoke test and build/push a Docker image.

## Quick start (local)

1. Create a virtual environment and install requirements:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

2. Generate a QR code (example):

```bash
python main.py --url "https://www.njit.edu/"
```

Generated PNG(s) will appear under `qr_codes/`.

## Docker (build & run)

Build locally (requires Docker installed):

```bash
docker build -t qr-code-generator-app:local .
docker run --rm -v "$(pwd)/qr_codes:/app/qr_codes" qr-code-generator-app:local python main.py --url "https://example.com/"
```

## GitHub Actions

There is a CI workflow in `.github/workflows/ci.yml` that:

- Runs a smoke test (installs dependencies and runs `python main.py --url ...`).
- Builds and pushes a Docker image using `docker/build-push-action` when secrets are configured.

Secrets required for the Docker push job:

- `DOCKERHUB_USERNAME` — your Docker Hub username
- `DOCKERHUB_TOKEN` — a Docker Hub access token or password

Add those under the repository Settings → Secrets → Actions before expecting the image push to succeed.

## Notes

- The workflow triggers on `push` and `pull_request` to the `main`/`master` branches.
- If you want to run only the test locally, `python main.py` is a fast way to verify functionality.

## Troubleshooting

- If `qr_codes/` is missing, create it: `mkdir -p qr_codes`.
- If Docker push fails on GitHub Actions, confirm the secrets are set and that the target tag/repository exists or the account has permission to create it.
