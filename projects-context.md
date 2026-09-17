# DevOps Practice Projects — Context & Learning Guide

This document accompanies the main `projects-roadmap.md` file.

The roadmap provides a high-level overview of the projects, while this document contains the **background knowledge, concepts, skills, terminology, commands and learning objectives required to complete each project**.

The purpose of these projects is not simply to produce working infrastructure.

The goal is to understand:

- what each technology does
- why it exists
- when it should be used
- how different DevOps technologies interact
- how to diagnose failures
- how to automate repetitive tasks
- how the same concepts map onto production cloud environments such as AWS

---

# Learning Approach

These projects should be completed using a **guided problem-solving approach**.

Rather than copying complete solutions, each project should follow the process:

```text
Learn concept
     ↓
Understand problem
     ↓
Attempt solution
     ↓
Inspect result
     ↓
Debug mistakes
     ↓
Improve solution
     ↓
Explain what happened
```

The objective is to be able to reproduce and explain the work without relying entirely on tutorials or generated solutions.

Using documentation, search engines and AI tools is encouraged, but they should primarily be used to:

- explain unfamiliar concepts
- clarify errors
- review attempted solutions
- provide hints
- explain command syntax
- suggest debugging approaches
- identify areas for improvement

They should not automatically complete every task.

---

# General DevOps Mental Model

A simplified DevOps workflow can be thought of as:

```text
Developer
    │
    ↓
Source Control
    │
    ↓
CI/CD
    │
    ↓
Build
    │
    ↓
Container
    │
    ↓
Infrastructure
    │
    ↓
Deployment
    │
    ↓
Running Application
    │
    ↓
Monitoring
```

Different tools solve different parts of this lifecycle.

Examples:

```text
Git                 → Source control

GitLab CI/CD        → Automation pipelines

Docker              → Application packaging

Terraform           → Infrastructure provisioning

AWS                 → Cloud infrastructure

Kubernetes          → Container orchestration

Prometheus          → Metrics collection

Grafana             → Monitoring dashboards
```

Later projects will connect these technologies together.

---

# Project 1 — Linux Server Lab

## Project Goal

Learn the Linux fundamentals required to administer and troubleshoot servers used in DevOps environments.

Linux is particularly important because a large proportion of:

- cloud servers
- containers
- Kubernetes nodes
- CI/CD runners
- infrastructure tooling
- web servers

run on Linux-based operating systems.

The objective is therefore not simply to learn Linux commands.

The objective is to become comfortable entering an unfamiliar Linux environment and answering questions such as:

```text
Where am I?

Who am I logged in as?

What machine am I connected to?

What files exist?

What processes are running?

What services are running?

How much CPU and memory are being used?

Where are the logs?

Who owns this file?

Why can't this user access something?

Is the machine connected to the network?

Why has an application stopped working?
```

---

# Project Scenario

Imagine that you have joined a company as a DevOps engineer.

You have been given access to a new Ubuntu server.

You have been asked to investigate and administer it.

Your responsibilities include:

- navigating the server
- understanding its filesystem
- managing files and directories
- creating users and groups
- managing permissions
- installing software
- inspecting running processes
- managing services
- investigating logs
- checking CPU usage
- checking memory usage
- checking disk usage
- investigating network configuration
- writing Bash scripts to automate common tasks

For this project, the server will initially run locally.

Later, the same concepts will be mapped onto AWS EC2.

---

# 1. Linux Environment

Because the development computer is running Windows, Linux can be run locally using **WSL2 — Windows Subsystem for Linux**.

A simplified architecture looks like:

```text
Windows
   │
   ↓
WSL2
   │
   ↓
Ubuntu Linux
```

This provides a Linux environment without requiring a separate physical machine or full traditional virtual machine.

In a cloud environment the equivalent concept might look like:

```text
AWS
 │
 ↓
EC2 Virtual Machine
 │
 ↓
Ubuntu Linux
```

The operating environment is different, but most Linux administration concepts remain the same.

---

# 2. Check the Linux Environment

Before beginning the project, determine whether WSL and Ubuntu are already installed.

From PowerShell investigate:

```powershell
wsl --status
```

and:

```powershell
wsl --list --verbose
```

Try to determine:

1. Whether WSL is installed
2. Which Linux distributions are installed
3. Whether Ubuntu is available
4. Whether the distribution uses WSL1 or WSL2

Do not worry about understanding every field immediately.

The important skill is learning to inspect the system before changing it.

---

# 3. Linux Command Structure

Many Linux commands follow the general pattern:

```text
command [options] [arguments]
```

For example:

```bash
ls -la /home
```

can be separated into:

```text
ls        command

-la       options

/home     argument
```

The command determines what operation should be performed.

Options modify the behaviour of the command.

Arguments normally specify what the command should operate on.

This pattern appears throughout Linux and many DevOps tools.

Examples later include:

```bash
docker run ...
```

```bash
kubectl get pods
```

```bash
terraform plan ...
```

```bash
aws s3 ls
```

Understanding command structure therefore becomes useful far beyond Linux itself.

---

# 4. Initial Linux Inspection

When entering an unfamiliar server, one of the first tasks is understanding the environment.

Investigate the following commands:

```bash
pwd
```

```bash
ls
```

```bash
ls -l
```

```bash
ls -a
```

```bash
ls -la
```

```bash
whoami
```

```bash
hostname
```

```bash
uname -a
```

Rather than immediately memorising definitions, run each command and inspect its output.

Try to determine what information each command provides.

Useful questions to consider:

```text
What does pwd appear to show?

What information does whoami return?

What does hostname identify?

What information does uname provide?

Why does ls -la show more information than ls?
```

---

# 5. Command Options

Linux commands often support many options.

For example:

```bash
ls -l
```

and:

```bash
ls -a
```

modify the normal behaviour of:

```bash
ls
```

Investigate what the following options mean:

```text
-l

-a
```

Then determine what happens when they are combined:

```bash
ls -la
```

A useful technique when encountering an unfamiliar command is to inspect its manual.

Linux commonly provides:

```bash
man <command>
```

For example:

```bash
man ls
```

Another common option is:

```bash
<command> --help
```

For example:

```bash
ls --help
```

Part of becoming comfortable with Linux is learning how to discover command behaviour without memorising every option.

---

# 6. Linux Filesystem

Windows commonly exposes separate filesystems using drive letters:

```text
C:\

D:\
```

Linux instead uses a single filesystem hierarchy beginning at:

```text
/
```

This is called the **root directory**.

A simplified Linux filesystem might look like:

```text
/
├── bin
├── etc
├── home
│   └── user
├── opt
├── root
├── tmp
├── usr
└── var
```

The `/` directory should not be confused with the **root user**.

They are separate concepts:

```text
/           → Root directory

root        → Administrative user
```

---

# 7. Important Linux Directories

You do not need to memorise the entire Linux filesystem immediately.

For this project, begin by understanding these directories:

```text
/home

/etc

/var

/tmp
```

A useful initial mental model is:

```text
/home
    User home directories

/etc
    System and application configuration

/var
    Changing runtime data such as logs

/tmp
    Temporary files
```

Later in the project, particular attention will be given to:

```text
/var/log
```

because logs are extremely important when diagnosing Linux servers.

Explore:

```bash
ls /
```

```bash
ls /home
```

```bash
ls /etc
```

```bash
ls /var
```

Do not modify system files yet.

The purpose at this stage is simply to become familiar with the filesystem.

---

# 8. Paths

Linux paths can be either:

```text
Absolute paths
```

or:

```text
Relative paths
```

An absolute path begins from:

```text
/
```

Example:

```text
/home/user/project
```

A relative path is interpreted from the current working directory.

Understanding the difference becomes important when working with:

- Bash scripts
- Docker
- Terraform
- CI/CD pipelines
- configuration files
- application deployments

During the exercises, pay attention to whether commands use absolute or relative paths.

---

# 9. Home Directory

Linux users generally have a home directory.

A common shorthand is:

```text
~
```

Investigate what directory `~` represents for your current user.

Try comparing:

```bash
pwd
```

with navigation involving:

```bash
cd ~
```

Try to understand the relationship between:

```text
~

/home/<username>
```

---

# 10. Working with Directories

Two fundamental commands are:

```bash
mkdir
```

and:

```bash
cd
```

Rather than memorising them immediately, infer their purpose from their names and behaviour.

The first project workspace should eventually look like:

```text
devops-linux-lab/
├── backups/
├── files/
├── logs/
└── scripts/
```

Create the workspace under your home directory:

```text
~/devops-linux-lab/
```

Try to determine the commands required to create this structure.

After navigating into the directory, verify your location using one of the commands introduced earlier.

---

# 11. Project Workspace

The directory structure will be used throughout Project 1.

```text
~/devops-linux-lab/
│
├── files/
│
│   └── Used for practising file operations
│
├── logs/
│
│   └── Used for practising log inspection
│
├── backups/
│
│   └── Used by future backup scripts
│
└── scripts/
    │
    └── Bash scripts created later in the project
```

Eventually the `scripts` directory may contain tools such as:

```text
scripts/
├── server_health.sh
├── backup.sh
└── cleanup_logs.sh
```

Do not build these scripts yet.

They will be introduced after the required Linux and Bash concepts have been learned.

---

# 12. First Linux Investigation Challenge

Using the commands introduced so far, investigate your Linux environment and determine:

### System

1. What Linux distribution am I running?
2. What Linux kernel am I running?
3. What is my current username?
4. What is the hostname of the machine?

### Filesystem

5. What is my current working directory?
6. What does `~` represent?
7. What is `/`?
8. What is `/home` used for?
9. What is `/etc` used for?
10. What is `/var` used for?

### Commands

11. What does `ls` do?
12. What does `ls -l` change?
13. What does `ls -a` change?
14. What happens when the options are combined as `ls -la`?

### Workspace

15. Create:

```text
~/devops-linux-lab/
```

16. Inside it create:

```text
files/
logs/
backups/
scripts/
```

17. Navigate into the project directory.

18. Verify that the directory structure exists.

---

# 13. Project 1 Skill Progression

Project 1 will gradually progress through the following topics:

```text
Linux Environment
        │
        ↓
Filesystem Navigation
        │
        ↓
Files and Directories
        │
        ↓
Users and Groups
        │
        ↓
Permissions and Ownership
        │
        ↓
Package Management
        │
        ↓
Processes
        │
        ↓
Services
        │
        ↓
Logs
        │
        ↓
CPU / Memory / Disk
        │
        ↓
Networking
        │
        ↓
SSH
        │
        ↓
Bash Automation
        │
        ↓
AWS EC2 Mapping
```

Each section should build upon knowledge from the previous sections.

---

# 14. Skills Practised

By the end of Project 1, the objective is to develop introductory practical knowledge of:

```text
Linux

Linux filesystem

Shell commands

Command-line navigation

Files and directories

Users

Groups

File ownership

Permissions

Package management

Processes

Services

Logs

CPU monitoring

Memory monitoring

Disk monitoring

Linux networking

SSH

Environment variables

Basic Bash scripting

Server troubleshooting

EC2 concepts
```

---

# 15. AWS Connection

The Linux skills learned in this project directly apply to AWS EC2.

Conceptually:

```text
Local Lab                    AWS

WSL Ubuntu          →        EC2 Ubuntu Instance

Linux User          →        EC2 Linux User

Linux Process       →        Process running on EC2

Linux Service       →        Service running on EC2

Linux Firewall      →        Host-level firewall

Networking          →        VPC networking

SSH                 →        SSH into EC2

Application Logs    →        EC2 application logs
```

Additional AWS infrastructure surrounds the server:

```text
AWS Account
    │
    ↓
Region
    │
    ↓
VPC
    │
    ↓
Subnet
    │
    ↓
Security Group
    │
    ↓
EC2 Instance
    │
    ↓
Ubuntu Linux
    │
    ↓
Applications / Services
```

These AWS concepts will be explored progressively in later projects.

The important idea for now is:

> Cloud infrastructure does not remove the need to understand operating systems.

A cloud server is still a server that must be configured, monitored and debugged.

---

# 16. Completion Criteria

Project 1 should not be considered complete simply because all commands have been executed.

Before moving on, I should be comfortable explaining:

- how to navigate through Linux
- the difference between relative and absolute paths
- what the Linux root directory is
- what a user home directory is
- how command options work
- how to investigate unfamiliar commands
- where important configuration files are commonly stored
- where logs are commonly stored
- how Linux relates to an AWS EC2 instance

I should also be capable of entering an unfamiliar Linux terminal and independently beginning to investigate the machine.

---

# Next Topic

The next stage of Project 1 will introduce:

```text
Linux files
Linux users
Linux groups
Permissions
Ownership
```

These concepts are fundamental to understanding how Linux controls who is allowed to read, modify and execute files on a server.