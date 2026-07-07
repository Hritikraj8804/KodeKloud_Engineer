# Instruction

The xFusionCorp Industries ML Platform team uses a lightweight shell-based CI pipeline to automate the build process for their Flask fraud-detection service.

The pipeline performs four stages:

1. Run unit tests
2. Build the Docker image
3. Tag the image with the current Git short SHA
4. Push the image to the private Docker registry

The provided `build.sh` script contained several wiring issues that prevented the pipeline from completing successfully.


# Solution

## Final `build.sh`

```bash
#!/bin/bash
# Shell-based CI pipeline for the ml-ci-app image.
#
# Stages:
#   1. Test
#   2. Build
#   3. Tag with Git SHA
#   4. Push to Private Registry
#
# Run from /root/code/ci/

set -euo pipefail

cd "$(dirname "$0")"

IMAGE="ml-ci-app"
REGISTRY="localhost:5555"

# -----------------------------
# Stage 1 - Run Tests
# -----------------------------
echo "[ci] stage 1/4 — running tests"

python3 -m pytest app/

# -----------------------------
# Stage 2 - Build Image
# -----------------------------
echo "[ci] stage 2/4 — building image"

docker build -t "$IMAGE:latest" app/

# -----------------------------
# Stage 3 - Tag Image
# -----------------------------
echo "[ci] stage 3/4 — tagging"

SHA=$(git -C app rev-parse --short HEAD)

TAGGED="$REGISTRY/$IMAGE:$SHA"

docker tag "$IMAGE:latest" "$TAGGED"

# -----------------------------
# Stage 4 - Push Image
# -----------------------------
echo "[ci] stage 4/4 — pushing"

docker push "$TAGGED"

echo "[ci] complete: $TAGGED"
```

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/ci
```

## Step 2 - Edit Script

```bash
vi build.sh
```

Fix the following:

* Change registry to **localhost:5555**
* Change pytest path to **app/**
* Replace `$GIT_SHA` with `$SHA`

## Step 3 - Make Script Executable

```bash
chmod +x build.sh
```

## Step 4 - Run the Pipeline

```bash
./build.sh
```

Expected output:

```text
[ci] stage 1/4 — running tests
==========================
3 passed
==========================

[ci] stage 2/4 — building image

Successfully built ...

[ci] stage 3/4 — tagging

[ci] stage 4/4 — pushing

latest: digest: ...

[ci] complete: localhost:5555/ml-ci-app:abc1234
```

<img width="707" height="292" alt="exec" src="https://github.com/user-attachments/assets/643ef965-e8e7-4a1d-9c4c-cd397efff8b4" />

# Verification

## Verify Local Image

```bash
docker images ml-ci-app
```

Expected:

```text
REPOSITORY    TAG

ml-ci-app     latest
```

<img width="736" height="106" alt="docker imag" src="https://github.com/user-attachments/assets/2e0fe847-ae89-406b-99c8-ba2d5d4157c1" />

## Verify Registry Catalog

```bash
curl http://localhost:5555/v2/_catalog
```

Expected:

```json
{
  "repositories": [
    "ml-ci-app"
  ]
}
```

<img width="742" height="131" alt="curl" src="https://github.com/user-attachments/assets/0c0fadd4-1880-4753-ac58-f59639c526fb" />

## Verify Git SHA Tag

Find the current Git SHA:

```bash
git -C app rev-parse --short HEAD
```

Example:

```text
4d72c1f
```

<img width="645" height="57" alt="git head" src="https://github.com/user-attachments/assets/5443d429-da55-4d10-a1d3-11c888d6c563" />

Now verify the registry:

```bash
curl http://localhost:5555/v2/ml-ci-app/tags/list
```

Expected:

```json
{
  "name":"ml-ci-app",
  "tags":[
    "4d72c1f"
  ]
}
```


# Fun Message 😄

*"A good CI pipeline doesn't just build containers—it gives every image a Git fingerprint, proving exactly which commit is ready for deployment! 🚀🐳"*
