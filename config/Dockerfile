FROM ubuntu:20.04

# Install necessary packages
RUN apt-get update && \
    apt-get install -y curl sudo apt-transport-https && \
    curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash - && \
    apt-get install -y nodejs && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create a user to run the agent
RUN useradd -m shalevv && \
    mkdir /azp && \
    chown -R shalevv:shalevv /azp

# Install Azure Pipelines Agent
WORKDIR /azp
RUN curl -LsS https://vstsagentpackage.azureedge.net/agent/2.193.1/vsts-agent-linux-x64-2.193.1.tar.gz | tar -xz

# Install dependencies
RUN ./bin/installdependencies.sh

# Switch to non-root user
USER shalevv

# Set entrypoint for agent
CMD ["./bin/Agent.Listener", "run"]
