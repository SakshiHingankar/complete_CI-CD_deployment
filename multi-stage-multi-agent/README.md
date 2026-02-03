# Multi-Agent Jenkins Pipeline

A Jenkins pipeline demonstrating the use of multiple Docker agents to run different stages in isolated containerized environments.

## Overview

This Jenkinsfile showcases how to use Docker-based agents in Jenkins to create isolated build environments for different technology stacks. It runs two stages in parallel, each using a different Docker container without requiring tools to be installed directly on the Jenkins server.

## How It Works

1. Pipeline starts with `agent none` (no default agent)
2. Each stage specifies its own Docker agent
3. Jenkins pulls the Docker image if not already available
4. Stage executes inside the container
5. Container is destroyed after stage completion
