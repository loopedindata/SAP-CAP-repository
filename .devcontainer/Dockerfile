# syntax=docker/dockerfile:1

ARG VARIANT="20"
FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:0-${VARIANT}

# Install useful OS tools + Cloud Foundry CLI + jq + zip/unzip
USER root

RUN apt-get update && \
    apt-get -y install --no-install-recommends \
        curl \
        git \
        sqlite3 \
        jq \
        zip \
        unzip \
        lsb-release \
        ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# Install Cloud Foundry CLI (latest)
RUN curl -L "https://packages.cloudfoundry.org/stable?release=linux64-binary&source=github" \
    | tar -zx && \
    mv cf /usr/local/bin/cf && \
    chmod +x /usr/local/bin/cf

# Optional: Install SAP MultiApps CF CLI plugin (for xs deploy)
RUN cf install-plugin -r CF-Community "multiapps" -f

# Optional: Install CF UAA CLI (used in BTP)
RUN curl -L https://github.com/cloudfoundry/uaa-cli/releases/latest/download/uaa-linux \
    -o /usr/local/bin/uaa && \
    chmod +x /usr/local/bin/uaa

RUN echo 'export PS1="\n[\u@\h] (\W)\n$ "' >> /home/node/.bashrc


# Back to normal Node user
USER node

# Install SAP CAP globally
RUN npm install -g @sap/cds-dk

# Install UI5 tooling (Fiori / UI5 projects)
RUN npm install -g @ui5/cli

# Optional: SAP Fiori tools (if you want local OData annotations & generators)
RUN npm install -g @sap-ux/fe-fpm-writer @sap-ux/ui5-tooling-modules

WORKDIR /home/node
