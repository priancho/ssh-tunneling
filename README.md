# SSH tunneling
How to tunnel intermediate server(s) to connect the end server with ssh.

## HowTo

- Caution!
  - If you use windows, use C:\Windows\System32\OpenSSH\ssh.exe instead of ssh for ProxyCommand section.

### 1. Create a public-private key pair

Create a public-private key pair on your working machine (e.g. laptop).

```bash
$ ssh-kengen
```

### 2. Set up password-less login

Add your public key into the .ssh/authorized_keys file in all intermediate and end servers.
Be carefult that .ssh (700) and .ssh/authorized_keys (644) have proper permissions.

### 3. Set up ssh tunneling config

Add the following configuration in your working machine's .ssh/config file.


```bash
Host intermediate_server_01
  HostName intermediate_server_01.xyz.com
  User myuser
Host intermediate_server_02
  HostName intermediate_server_02.xyz.com
  User myuser
  StrictHostKeyChecking no
  ProxyCommand ssh -W %h:%p intermediate_server_01
Host end_server
  HostName end_server.xyz.com
  User myuser
  StrictHostKeyChecking no
  ProxyCommand ssh -W %h:%p intermediate_server_02
```
