# ⚔️ ludus-atomic-red-team


This repository helps you set up an automated [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team) testing environment using [Ludus](https://ludus.cloud/). The environment allows you to safely execute and test atomic tests while having the ability to revert machines back to clean snapshots.

## 📋 Contents

- [⚔️ ludus-atomic-red-team](#️-ludus-atomic-red-team)
  - [📋 Contents](#-contents)
  - [🔍 What is Ludus?](#-what-is-ludus)
  - [⚙️ Prerequisites](#️-prerequisites)
  - [🚀 Quick Start](#-quick-start)
    - [1. Clone the official Ludus repository](#1-clone-the-official-ludus-repository)
    - [2. Add and build desired templates](#2-add-and-build-desired-templates)
    - [3. Clone this repository](#3-clone-this-repository)
    - [4. Install required Ansible collections and roles](#4-install-required-ansible-collections-and-roles)
    - [5. Deploy your range](#5-deploy-your-range)
    - [6. Start testing](#6-start-testing)
  - [🤝 Contributing](#-contributing)
  - [📚 Additional Resources](#-additional-resources)

## 🔍 What is Ludus?

Ludus is a powerful platform for building and managing cyber ranges - isolated environments perfect for security testing and development. It provides:

- ✅ Easy-to-use infrastructure as code
- ✅ Snapshot and revert capabilities
- ✅ Multiple OS template support
- ✅ Isolated network environments
- ✅ Easily add/remove users

## ⚙️ Prerequisites

1. A working Ludus installation
   - Follow the [Ludus Deployment Guide](https://docs.ludus.cloud/docs/category/deployment-options) to set up your instance

## 🚀 Quick Start

Ludus comes with several built-in OS templates:

```
+------------------------------------+-------+
|              TEMPLATE              | BUILT |
+------------------------------------+-------+
| debian-11-x64-server-template      | FALSE |
| debian-12-x64-server-template      | TRUE  |
| kali-x64-desktop-template          | TRUE  |
| win11-22h2-x64-enterprise-template | TRUE  |
| win2022-server-x64-template        | TRUE  |
+------------------------------------+-------+
```

You can add more OS templates (Rocky Linux, Ubuntu, other Windows versions, etc.) from the [official Ludus repository](https://gitlab.com/badsectorlabs/ludus):

### 1. Clone the official Ludus repository

```bash
git clone https://gitlab.com/badsectorlabs/ludus
cd templates
```

### 2. Add and build desired templates

```bash
# Add a new template (example: Rocky Linux 9)
ludus templates add -d rocky-9-x64-server

# Build multiple templates at once
ludus templates build -n debian-12-x64-server-template,rocky-9-x64-server-template,win11-22h2-x64-enterprise-template,win2022-server-x64-template
```

### 3. Clone this repository

```bash
git clone https://github.com/cyberbuff/ludus-atomic-red-team
cd ludus-atomic-red-team
```

### 4. Install required Ansible collections and roles

```bash
# Install role prerequisites
ludus ansible collection add git+https://github.com/CowDogMoo/ansible-collection-workstation.git,main

# Install Atomic Red Team and its prerequisites
ludus ansible role add -d ./setup-atomic-red-team
```

### 5. Deploy your range

The `atomic-red-team-range.yaml` sample configuration file contains the configuration for your testing environment. Customize the following according to your needs:

- Number and types of machines
- Network configuration
- [Installed tools and packages](https://gitlab.com/badsectorlabs/ludus/-/blob/main/docs/docs/configuration.mdx?ref_type=heads#L62-72)

```bash
# Apply the sample configuration
ludus range config set -f atomic-red-team-range.yaml

# Deploy the range
ludus range deploy

# Monitor deployment
ludus range logs -f
```

### 6. Start testing

When you run this command, ludus will capture a snapshot of the machines in your range (depending on whether testing.snapshot is set to `true`).
```bash
ludus testing start
```

When you have finished running the tests, stop the testing to revert back to the snapshot state.

```bash
ludus testing stop
```


## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. Fork the repository
2. Create a feature branch
3. Submit a pull request with your changes
4. Report issues or suggest improvements

## 📚 Additional Resources

- [Ludus Documentation](https://docs.ludus.cloud/)
