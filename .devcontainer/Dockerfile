# Use an appropriate base image; e.g. Ubuntu or node image
FROM node:20-slim

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    # any other tools you need, e.g. build tools
    && rm -rf /var/lib/apt/lists/*

# Install Claude Code globally
# you may need to adjust permission or PATH depending on your base
RUN npm install -g @anthropic-ai/claude-code

# Set workdir
WORKDIR /workspace

# If you want: copy config files for Claude (if needed), or mount volumes later
