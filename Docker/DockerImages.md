# Docker Images and Layers

## Goals
- Document the basics of Docker images and layers
- Rebuild the Docker image for Laravel application and show impact

---

## Dockerfile Basics

A Dockerfile contains **instructions** and **arguments** that define how to build a Docker image.

### Basic Dockerfile Structure

```dockerfile
# Start from a base image
FROM ubuntu

# Install dependencies
RUN apt-get update
RUN apt-get install python

# Install Python packages
RUN pip install flask
RUN pip install flask-mysql

# Copy source code to the container
COPY . /opt/source-code

# Specify the entrypoint for the container
ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```

### Common Dockerfile Instructions

| Instruction | Purpose |
|------------|---------|
| `FROM` | Start from a base image or another image |
| `RUN` | Execute commands (install dependencies, etc.) |
| `COPY` | Copy files from host to container |
| `ENTRYPOINT` | Specify the command to run when container starts |

---

## Layer Architecture

Docker images are built in **layers**. Each instruction in a Dockerfile creates a new layer.

### Example: Layered Dockerfile

```dockerfile
# Layer 1: Base Ubuntu Layer (120MB)
FROM ubuntu

# Layer 2: APT Package Installation (306MB)
RUN apt-get update && apt-get install python

# Layer 3: Pip Package Installation (6.3MB)
RUN pip install flask && pip install flask-mysql

# Layer 4: Source Code (229KB)
COPY . /opt/source-code

# Layer 5: Entrypoint (0B)
ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```

### Layer Benefits

- **Caching**: Unchanged layers are reused, speeding up builds
- **Efficiency**: Only modified layers need to be rebuilt
- **Storage**: Shared layers between images save disk space

### Best Practices

1. **Combine RUN commands** - Use `&&` to reduce layers:
   ```dockerfile
   RUN apt-get update && apt-get install python
   ```

2. **Order matters** - Place frequently changing instructions (like `COPY`) near the end

3. **Minimize layer size** - Clean up in the same layer:
   ```dockerfile
   RUN apt-get update && apt-get install python && apt-get clean
   ```

---

## Inspecting Image Layers

### Docker History Command

View the breakdown of image size and layers:

```bash
docker history <image_name>
```

**Example output:**
```
IMAGE          CREATED        CREATED BY                                      SIZE
abc123def456   2 hours ago    ENTRYPOINT FLASK_APP=/opt/source-code/app.py    0B
def456ghi789   2 hours ago    COPY . /opt/source-code                         229KB
ghi789jkl012   2 hours ago    RUN pip install flask && pip install flask-m…   6.3MB
jkl012mno345   2 hours ago    RUN apt-get update && apt-get install python    306MB
mno345pqr678   3 days ago     FROM ubuntu                                     120MB
```



docker buildx build --platform linux/amd64 --no-cache -t gateway:intial --load .

dive gateway:intial       