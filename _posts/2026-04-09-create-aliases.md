---
layout: default
title: "How to Create Useful Shell Aliases"
date: 2026-04-09
categories: [shell, productivity]
---

Shell aliases and functions can significantly improve your command-line productivity by allowing you to execute complex commands with simple shortcuts. In this post, I'll show you how to create two useful aliases: one for listing open ports and another for freeing up occupied ports.

## What are Shell Aliases and Functions?

Aliases are shortcuts for commands, while functions allow for more complex logic including parameters. Both are defined in your shell's configuration file (`.bashrc` for Bash, `.zshrc` for Zsh).

## How to Add Aliases to Your Shell

1. Open your shell configuration file:
   - For Bash: `nano ~/.bashrc` or `nano ~/.bash_profile`
   - For Zsh: `nano ~/.zshrc` or `~/.zprofile`

2. Add the alias or function definitions at the end of the file.

3. Save and exit the editor.

4. Reload your configuration:
   - `source ~/.bashrc` or `source ~/.zshrc`

## Useful Aliases for Port Management

### Alias for Listing Open Ports

```bash
alias ports="lsof -iTCP -sTCP:LISTEN -P -n"
```

This alias uses `lsof` (list open files) to display all TCP ports that are currently listening. The flags mean:
- `-iTCP`: Show only TCP connections
- `-sTCP:LISTEN`: Show only listening sockets
- `-P`: Don't convert port numbers to names
- `-n`: Don't convert IP addresses to hostnames

Usage: Simply type `ports` in your terminal.

### Function for Freeing Up Ports

```bash
function freeport() {
  if [ -z "$1" ]; then
    echo "Usage: freeport <port>"
    return 1
  fi
  kill -9 "$(lsof -tiTCP:$1 -sTCP:LISTEN)"
}
```

This function takes a port number as an argument and forcefully terminates the process using that port. The `lsof` command finds the process ID, and `kill -9` sends a SIGKILL signal.

Usage: `freeport 8080` to free port 8080.

**Warning:** Use `freeport` carefully as it will terminate processes without saving data.

## Full Sample

```
alias ls="ls --color"
alias ll="ls -alh"
alias cls="clear"

alias ssh-my-server-with-port-forward="ssh -i ~/my-ssh-key.pem -L 8080:localhost:80 my-user-name@my-server-ip.com"

alias myip="curl checkip.amazonaws.com"
alias myip2="curl ifconfig.me"
alias myip3="curl api.ipify.org"

# DNS
alias flush-dns="sudo killall -HUP mDNSResponder; sudo dscacheutil -flushcache; echo DNS cache flushed"
alias mongo_srv_lookup='function _mongo_srv_lookup(){ nslookup -type=SRV _mongodb._tcp.$1; }; _mongo_srv_lookup'

alias ports="lsof -iTCP -sTCP:LISTEN -P -n"
function freeport() {
  if [ -z "$1" ]; then
    echo "Usage: freeport <port>"
    return 1
  fi
  kill -9 "$(lsof -tiTCP:$1 -sTCP:LISTEN)"
}

# Java
alias java-list="/usr/libexec/java_home -V"
alias java-27="export JAVA_HOME=`/usr/libexec/java_home -v 27`; java -version"
alias java-25="export JAVA_HOME=`/usr/libexec/java_home -v 25`; java -version"
alias java-21="export JAVA_HOME=`/usr/libexec/java_home -v 21`; java -version"
alias java-17="export JAVA_HOME=`/usr/libexec/java_home -v 17.0.13`; java -version"
alias java-11="export JAVA_HOME=`/usr/libexec/java_home -v 11`; java -version"
alias java-17-zulu="export JAVA_HOME=/Users/mohammed.fayed/Library/Java/JavaVirtualMachines/zulu-17.jdk/Contents/Home; java -version"


# Ollama
export OLLAMA_HOST=0.0.0.0
alias openwebui="docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main; echo 'Open-WebUI is running at http://localhost:3000'"

# Docker
export DOCKER_DEFAULT_PLATFORM=linux/amd64

alias docker-arm="DOCKER_DEFAULT_PLATFORM=linux/arm64 docker"
alias docker-amd="DOCKER_DEFAULT_PLATFORM=linux/amd64 docker"
alias ubuntu="docker run -it --rm ubuntu:latest bash"


# DOTNET
export DOTNET_CLI_TELEMETRY_OPTOUT=1


```

## Conclusion

These aliases can save you time when working with network services and debugging port conflicts. Add them to your shell configuration and enjoy a more efficient command-line experience!