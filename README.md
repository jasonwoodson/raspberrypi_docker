# raspberrypi_docker
Setting Up Raspberry Pi Docker 32/64 bit
Installing Docker
Installing Docker CE (Community Edition) on the Raspberry Pi OS requires running just a few commands.

The best way to install Docker is to fetch it from the official Docker repositories, so to ensure that you’re always running the latest version.

To install Docker CE on Raspberry Pi OS, both 32-bit and 64-bit, run:

# Install some required packages first
sudo apt update
sudo apt install -y \
     apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common

# Get the Docker signing key for packages
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -

# Add the Docker official repos
echo "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
     $(lsb_release -cs) stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list

# Install Docker
sudo apt update
sudo apt install -y --no-install-recommends \
    docker-ce \
    cgroupfs-mount
Done! At this point, we just need to run two more commands to have the Docker service started and automatically launched at boot.

sudo systemctl enable --now docker
Now that we have Docker running, we can test it by running the “hello world” image:

sudo docker run --rm hello-world
If everything is working, the command above will output something similar to:

Docker images for 32 and 64 bit ARM
On Docker Hub, the number of images for the ARM architecture used by the Raspberry Pi is growing by the day. Even though the majority of images are still only available for the x86 architecture (used by Intel and AMD CPUs, for example), the amount of ARM-compatible images is increasing steadily.

Additionally, because of the growing popularity of 64-bit ARM in certain cloud providers, it might be especially easier to find 64-bit Docker containers.

When searching for an image on Docker Hub, you can filter by operating system and architecture, where “ARM” refers to the 32-bit variant.

In the Docker ecosystem, 64-bit ARM images are called arm64 or arm64/v8.

Likewise, 32-bit images for Raspberry Pi OS are labeled as armhf, armv7, or arm/v7.

Using Docker Compose
Lastly, let’s look at how to add Docker Compose.

As of Docker Compose v2, the application does not have a dependency on Python, and pre-built binaries are available for all ARM-based systems.

First, find the latest version from the releases page; as of writing, that is 2.1.1. Then run these commands:

# Replace with the latest version from https://github.com/docker/compose/releases/latest
DOCKER_COMPOSE_VERSION="2.1.1"
# For 64-bit OS use:
DOCKER_COMPOSE_ARCH="aarch64"
# For 32-bit OS use:
DOCKER_COMPOSE_ARCH="armv7"

sudo curl -L "https://github.com/docker/compose/releases/download/v${DOCKER_COMPOSE_VERSION}/docker-compose-linux-${DOCKER_COMPOSE_ARCH}" -o /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
With this, you now have a complete Raspberry Pi mini-server running Docker and ready to accept your containers.




