# Use the official Ubuntu base image
FROM ubuntu:20.04

# Install necessary dependencies
RUN apt-get update && \
    apt-get install -y curl sudo apt-transport-https && \
    curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash - && \
    apt-get install -y nodejs && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create the /azp directory and set it as the working directory
RUN mkdir /azp
WORKDIR /azp

# Download and extract the Azure Pipelines agent package
RUN curl -LsS https://vstsagentpackage.azureedge.net/agent/2.193.1/vsts-agent-linux-x64-2.193.1.tar.gz | tar -xz

# Install additional dependencies required by the agent
RUN ./bin/installdependencies.sh

# Create a non-root user and grant permissions
RUN useradd -m azureagent && \
    chown -R azureagent:azureagent /azp && \
    chmod -R u+rw /azp

# Switch to the non-root user
USER azureagent

# Default command to configure and run the agent
CMD ["bash", "-c", "./config.sh --unattended \
    --agent ${AZP_AGENT_NAME} \
    --url ${AZP_URL} \
    --auth PAT \
    --token ${AZP_TOKEN} \
    --pool ${AZP_POOL} \
    --replace \
    --acceptTeeEula && ./bin/Agent.Listener run"]

