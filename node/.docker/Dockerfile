ARG NODE_IMAGE=node:18-slim

FROM ${NODE_IMAGE}

RUN apt-get update \
    && apt-get install -y build-essential git \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Create app directory
WORKDIR /workspace

# Copy files
COPY package.json yarn.lock* package-lock.json* ./

# Install dependencies
RUN yarn --frozen-lockfile

# Default to console
CMD ["yarn", "start"]
