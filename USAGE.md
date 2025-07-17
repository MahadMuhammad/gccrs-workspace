# Commands for running the GCC Rust workspace using docker on macOS

## Building the Docker Image

To build the Docker image for the GCC Rust workspace, run the following command in the terminal:

```bash
docker build -t mahadmuhammad/gccrs-workspace .
```

Uploading the image to Docker Hub can be done with:

```bash
docker buildx build --platform linux/amd64,linux/arm64 -t mahadmuhammad/gccrs-workspace:0.1 --push .
```

## Running the Docker Container

To run the Docker container with the built image, use the following command:

```bash
docker run -it --rm -p 2200:22 -v $(pwd)/rust-gcc:/home/rust-gcc mahadmuhammad/gccrs-workspace:0.1
```

For me, if I am in the `gccrs-workspace` directory, I can use:

```bash
docker run -it --rm -p 2200:22 -v ../../rust-gcc:/home/rust-gcc mahadmuhammad/gccrs-workspace:0.1
```

For connecting to running container, you can use:

```bash
ssh -p 2200 workspace@localhost
```

When prompted for a password, use `workspace`. This is the default user created in the Dockerfile.

This command maps port 2200 on your host to port 22 in the container and mounts the `rust-gcc` directory from your current working directory to `/home/rust-gcc` in the container.

## Using Docker Compose

Alternatively, you can use Docker Compose to manage the container. First, ensure you have a `docker-compose.yml` file in your project directory. The file should look like this:

```yaml
version: "3.9"
services:
  gccrs-workspace:
    image: mahadmuhammad/gccrs-workspace
    tty: true
    volumes:
      - ./rust-gcc:/home/rust-gcc:rw
    ports:
      - "2200:22"
```

To start the container using Docker Compose, run:

```bash
docker-compose up -d
```

To stop the container, use:

```bash
docker-compose down
```
