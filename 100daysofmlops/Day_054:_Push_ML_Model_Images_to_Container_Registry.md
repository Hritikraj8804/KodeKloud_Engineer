# Instruction

The xFusionCorp Industries ML Platform team maintains a private Docker registry for internal deployments. The goal is to build the **fraud-detector** image, tag it for the private registry, and push it so downstream systems can pull it.

The provided `push.sh` script built and tagged the image correctly, but **never pushed it**, and it pointed to the wrong registry port.



# Solution

Update the registry address and push the image.


# Solution Steps

## Final `push.sh`

```bash
#!/bin/bash
# Build the fraud-detector image and publish it to the local
# private registry so downstream clusters can pull by tag.
#
# Run from /root/code/ml-registry/.
set -euo pipefail

cd "$(dirname "$0")"

IMAGE="fraud-detector:v1"
REGISTRY="localhost:5555"

docker build -t "$IMAGE" .

docker tag "$IMAGE" "$REGISTRY/$IMAGE"

docker push "$REGISTRY/$IMAGE"

echo "Successfully pushed $REGISTRY/$IMAGE"
```


## Step 1 - Move to Project

```bash
cd /root/code/ml-registry
```

## Step 2 - Edit Script

```bash
vi push.sh
```

Make the following changes:

* Change registry port from **5000 → 5555**
* Add the `docker push` command

## Step 3 - Make Sure Script is Executable

```bash
chmod +x push.sh
```

## Step 4 - Run Script

```bash
./push.sh
```

Expected output:

```text
Successfully tagged localhost:5555/fraud-detector:v1

The push refers to repository [localhost:5555/fraud-detector]

...

Successfully pushed localhost:5555/fraud-detector:v1
```

<img width="936" height="307" alt="exec" src="https://github.com/user-attachments/assets/f0dc2ae6-3d92-4267-87ef-a478d95c7cfd" />

# Verification

## Verify Local Image

```bash
docker images fraud-detector:v1
```

Expected:

```text
REPOSITORY         TAG

fraud-detector     v1
```

## Verify Registry is Reachable

```bash
curl -s http://localhost:5555/v2/
```

Expected:

```json
{}
```

## Verify Repository Exists

```bash
curl http://localhost:5555/v2/_catalog
```

Expected:

```json
{
  "repositories": [
    "fraud-detector"
  ]
}
```

## Verify Tag

```bash
curl http://localhost:5555/v2/fraud-detector/tags/list
```

Expected:

```json
{
  "name": "fraud-detector",
  "tags": [
    "v1"
  ]
}
```

<img width="1032" height="337" alt="Verify" src="https://github.com/user-attachments/assets/04be2aaf-8a07-4899-bcdb-da0df68336d0" />

# Fun Message 😄

*"Building an image is only half the journey. Until you `docker push`, your registry is just an empty warehouse waiting for its first package! 📦🐳"*
