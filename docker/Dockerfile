FROM ubuntu:22.04

# Set environment variable to avoid interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Install required packages
RUN apt-get update && apt-get install -y \
    openssh-server \
    sudo \
    curl \
    wget \
    python3 \
    python3-pip \
    net-tools \
    iproute2 \
    gnupg \
    && apt-get clean

# Create user "ansible" with passwordless sudo privileges
RUN useradd -m -s /bin/bash ansible && \
    echo 'ansible ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Prepare SSH server runtime directory
RUN mkdir /var/run/sshd

# Expose SSH port
EXPOSE 22

# Add public SSH key for user "ansible"
RUN mkdir -p /home/ansible/.ssh
COPY test_key.pub /home/ansible/.ssh/authorized_keys
RUN chown -R ansible:ansible /home/ansible/.ssh && \
    chmod 700 /home/ansible/.ssh && \
    chmod 600 /home/ansible/.ssh/authorized_keys

# Start SSH server in foreground mode (required for Docker)
CMD ["/usr/sbin/sshd", "-D"]