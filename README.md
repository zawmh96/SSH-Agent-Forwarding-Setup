# SSH Agent Forwarding Setup Guide

SSH Agent Forwarding allows you to use your local machine's SSH keys when working with remote servers, which can be useful for accessing other servers from a remote machine without copying your private keys around.

## Prerequisites

- **SSH Client:** Ensure you have SSH installed on your local machine. Most Unix-based systems (Linux, macOS) come with SSH pre-installed. Windows users can use the built-in OpenSSH client or an alternative like PuTTY.
- **SSH Key Pair:** You should already have an SSH key pair generated on your local machine.

## Method 1: Using SSH Config File (Persistent)

**Step 1**: Start the SSH Agent
Before forwarding your SSH key, you need to ensure that the SSH agent is running on your local machine.

For Linux/macOS:
Open your terminal and run:
```
eval "$(ssh-agent -s)"
```
This command starts the SSH agent and prints the process ID.
For Windows (using Git Bash):
Start the SSH agent using the following command:
```
eval $(ssh-agent -s)
```

**Step 2**: Add Your SSH Key to the Agent
Once the SSH agent is running, add your SSH private key to the agent.
```
ssh-add <ssh-key>
```
If your SSH key is named something different or located elsewhere, adjust the path accordingly.

**Step 3**: Enable SSH Agent Forwarding in Your SSH Config
To enable SSH Agent Forwarding, you can configure it globally or on a per-host basis in your SSH configuration file.

Edit (or create) the SSH config file:
```
vi ~/.ssh/config
```
Add the following configuration:

```
Host server1
  ForwardAgent yes
  User your_username
  HostName server1-ip-address

Host server2
    HostName server2-ip-address
    User your_username
```
Replace **server-x ip address** with your remote **server's IP address** and **your_username** with **your actual username** on the remote server.   
If you want to enable forwarding for all hosts, you can use Host *.

**Step 4**: Connect to the Remote Server from local machine.  
Now you can SSH into your remote server with SSH Agent Forwarding enabled:
```
ssh your_username@server1-ip-address
```

**Step 5**: Connect to the Server 2 from Server 1.  
You can login from Remote Server 1 to Remote Server 2, Remote Server 1 acts as an intermediary.  
But the authentication still occurs using the SSH keys on your local machine via the forwarded agent.
```
ssh your_username@server2-ip-address
```

## Method 2: Using ssh -A (Ad-Hoc)

If you want to enable SSH Agent Forwarding for a single SSH session without modifying your SSH configuration file, you can use the -A option with the ssh command.
```
ssh -A your_username@server1-ip-address
ssh your_username@server2-ip-address
```
