# SSH 连接不上 Github


<!--more-->

## 问题描述
在修改自己的博客时，想把代码push上去，却失败，目前自我感觉与VPN有关，
- 在TUN Mode下的问题是
```bash
kex_exchange_identification: Connection closed by remote host
Connection closed by 20.205.243.166 port 22
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```
- 在System Proxy下的问题是
```bash
OpenSSH_8.9p1 Ubuntu-3ubuntu0.10, OpenSSL 3.0.2 15 Mar 2022
debug1: Reading configuration data /home/tieyi/.ssh/config
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: include /etc/ssh/ssh_config.d/*.conf matched no files
debug1: /etc/ssh/ssh_config line 21: Applying options for *
ssh: Could not resolve hostname github.com: Temporary failure in name resolution
```
## Debug过程
测试ssh的命令，`ssh -vT git@github.com` ，只是加上了`-v`选项，可以打印日志
```bash
OpenSSH_8.9p1 Ubuntu-3ubuntu0.10, OpenSSL 3.0.2 15 Mar 2022
debug1: Reading configuration data /home/tieyi/.ssh/config
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: include /etc/ssh/ssh_config.d/*.conf matched no files
debug1: /etc/ssh/ssh_config line 21: Applying options for *
debug1: Connecting to github.com [20.205.243.166] port 22.
debug1: Connection established.
debug1: identity file /home/tieyi/.ssh/id_rsa type -1
debug1: identity file /home/tieyi/.ssh/id_rsa-cert type -1
debug1: identity file /home/tieyi/.ssh/id_ecdsa type -1
debug1: identity file /home/tieyi/.ssh/id_ecdsa-cert type -1
debug1: identity file /home/tieyi/.ssh/id_ecdsa_sk type -1
debug1: identity file /home/tieyi/.ssh/id_ecdsa_sk-cert type -1
debug1: identity file /home/tieyi/.ssh/id_ed25519 type 3
debug1: identity file /home/tieyi/.ssh/id_ed25519-cert type -1
debug1: identity file /home/tieyi/.ssh/id_ed25519_sk type -1
debug1: identity file /home/tieyi/.ssh/id_ed25519_sk-cert type -1
debug1: identity file /home/tieyi/.ssh/id_xmss type -1
debug1: identity file /home/tieyi/.ssh/id_xmss-cert type -1
debug1: identity file /home/tieyi/.ssh/id_dsa type -1
debug1: identity file /home/tieyi/.ssh/id_dsa-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
kex_exchange_identification: Connection closed by remote host
Connection closed by 20.205.243.166 port 22
```
可见一直到寻找私钥的这个过程，都还是可以实现的

## 解决办法
根据这篇解答，[github - git pull encounters kex_exchange_identification: Connection closed by remote host - Stack Overflow](https://stackoverflow.com/questions/74469777/git-pull-encounters-kex-exchange-identification-connection-closed-by-remote-hos)，我在~/.ssh/config后面**加上**了
```bash
Host github.com
 Hostname ssh.github.com
 Port 443
```
并将Clash调整为**TUN Mode**，才可以成功。（System Proxy还是不行）

成功后的结果：
```bash
tieyi@LAPTOP-H67RCA14:~/codespace/site$ ssh -vT git@github.com
OpenSSH_8.9p1 Ubuntu-3ubuntu0.10, OpenSSL 3.0.2 15 Mar 2022
debug1: Reading configuration data /home/tieyi/.ssh/config
debug1: /home/tieyi/.ssh/config line 12: Applying options for github.com
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: include /etc/ssh/ssh_config.d/*.conf matched no files
debug1: /etc/ssh/ssh_config line 21: Applying options for *
debug1: Connecting to ssh.github.com [20.205.243.160] port 443.
debug1: Connection established.
debug1: identity file /home/tieyi/.ssh/id_rsa type -1
debug1: identity file /home/tieyi/.ssh/id_rsa-cert type -1
debug1: identity file /home/tieyi/.ssh/id_ecdsa type -1
debug1: identity file /home/tieyi/.ssh/id_ecdsa-cert type -1
debug1: identity file /home/tieyi/.ssh/id_ecdsa_sk type -1
debug1: identity file /home/tieyi/.ssh/id_ecdsa_sk-cert type -1
debug1: identity file /home/tieyi/.ssh/id_ed25519 type 3
debug1: identity file /home/tieyi/.ssh/id_ed25519-cert type -1
debug1: identity file /home/tieyi/.ssh/id_ed25519_sk type -1
debug1: identity file /home/tieyi/.ssh/id_ed25519_sk-cert type -1
debug1: identity file /home/tieyi/.ssh/id_xmss type -1
debug1: identity file /home/tieyi/.ssh/id_xmss-cert type -1
debug1: identity file /home/tieyi/.ssh/id_dsa type -1
debug1: identity file /home/tieyi/.ssh/id_dsa-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
debug1: Remote protocol version 2.0, remote software version babeld-e0f59911d
debug1: compat_banner: no match: babeld-e0f59911d
debug1: Authenticating to ssh.github.com:443 as 'git'
debug1: load_hostkeys: fopen /home/tieyi/.ssh/known_hosts2: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: curve25519-sha256
debug1: kex: host key algorithm: ssh-ed25519
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
debug1: SSH2_MSG_KEX_ECDH_REPLY received
debug1: Server host key: ssh-ed25519 SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU
debug1: load_hostkeys: fopen /home/tieyi/.ssh/known_hosts2: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory
debug1: Host '[ssh.github.com]:443' is known and matches the ED25519 host key.
debug1: Found key in /home/tieyi/.ssh/known_hosts:12
debug1: ssh_packet_send2_wrapped: resetting send seqnr 3
debug1: rekey out after 134217728 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: ssh_packet_read_poll2: resetting read seqnr 3
debug1: SSH2_MSG_NEWKEYS received
debug1: rekey in after 134217728 blocks
debug1: Will attempt key: /home/tieyi/.ssh/id_rsa 
debug1: Will attempt key: /home/tieyi/.ssh/id_ecdsa 
debug1: Will attempt key: /home/tieyi/.ssh/id_ecdsa_sk 
debug1: Will attempt key: /home/tieyi/.ssh/id_ed25519 ED25519 SHA256:X+pyeImgTwh8EcDJNxfmBmiXpu+9IVKp05NF7X8S5qk
debug1: Will attempt key: /home/tieyi/.ssh/id_ed25519_sk 
debug1: Will attempt key: /home/tieyi/.ssh/id_xmss 
debug1: Will attempt key: /home/tieyi/.ssh/id_dsa 
debug1: SSH2_MSG_EXT_INFO received
debug1: kex_input_ext_info: server-sig-algs=<ssh-ed25519-cert-v01@openssh.com,ecdsa-sha2-nistp521-cert-v01@openssh.com,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp256-cert-v01@openssh.com,sk-ssh-ed25519-cert-v01@openssh.com,sk-ecdsa-sha2-nistp256-cert-v01@openssh.com,rsa-sha2-512-cert-v01@openssh.com,rsa-sha2-256-cert-v01@openssh.com,ssh-rsa-cert-v01@openssh.com,sk-ssh-ed25519@openssh.com,sk-ecdsa-sha2-nistp256@openssh.com,ssh-ed25519,ecdsa-sha2-nistp521,ecdsa-sha2-nistp384,ecdsa-sha2-nistp256,rsa-sha2-512,rsa-sha2-256,ssh-rsa>
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Trying private key: /home/tieyi/.ssh/id_rsa
debug1: Trying private key: /home/tieyi/.ssh/id_ecdsa
debug1: Trying private key: /home/tieyi/.ssh/id_ecdsa_sk
debug1: Offering public key: /home/tieyi/.ssh/id_ed25519 ED25519 SHA256:X+pyeImgTwh8EcDJNxfmBmiXpu+9IVKp05NF7X8S5qk
debug1: Server accepts key: /home/tieyi/.ssh/id_ed25519 ED25519 SHA256:X+pyeImgTwh8EcDJNxfmBmiXpu+9IVKp05NF7X8S5qk
Authenticated to ssh.github.com ([20.205.243.160]:443) using "publickey".
debug1: channel 0: new [client-session]
debug1: Entering interactive session.
debug1: pledge: filesystem
debug1: client_input_global_request: rtype hostkeys-00@openssh.com want_reply 0
debug1: client_input_hostkeys: searching /home/tieyi/.ssh/known_hosts for [ssh.github.com]:443 / (none)
debug1: client_input_hostkeys: searching /home/tieyi/.ssh/known_hosts2 for [ssh.github.com]:443 / (none)
debug1: client_input_hostkeys: hostkeys file /home/tieyi/.ssh/known_hosts2 does not exist
debug1: client_input_hostkeys: host key found matching a different name/address, skipping UserKnownHostsFile update
debug1: Sending environment.
debug1: channel 0: setting env LANG = "C.UTF-8"
debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
Hi tearizz! You've successfully authenticated, but GitHub does not provide shell access.
debug1: channel 0: free: client-session, nchannels 1
Transferred: sent 2180, received 2608 bytes, in 0.7 seconds
Bytes per second: sent 3061.0, received 3662.0
debug1: Exit status 1
```


---

> 作者: tearizz  
> URL: http://localhost:1313/posts/3ade6a3/  

