# ssh_tunnel

client ------->boss(tunnel)------->target

This is my experience of using ssh for connecting a computer without static ip address from a remote computer\
This is required to manage projects in server (serving in LAN) with no static ip to use

Requirement for setup and demo:
- Linux in with an ssh server with static IP ( call it boss.com )
- Linux in with an ssh/web server with no static IP. (call it target)
- Linux / Android phone with JuiceSSH (call it client)


ssh
===
apt install openssh-server\
edit /etc/ssh/sshd_config\
<code>PermitRootLogin yes</code>\
also ensure following in /etc/ssh/sshd_config\
<code>GatewayPorts yes</code>\
service ssh restart

run following in target computer to use boss.com:1008 as web address for target \
<code>ssh -R 1008:127.0.0.1:80 root@boss.com</code>\
Now you can excess target web server as boot.com:1008 from any device (try with browser from your mobile device)

run following in target computer to connect target computer via ssh(at 2048 port)\
<code>ssh -R 1008:127.0.0.1:2048 root@boss.com</code>\
Now you can excess target computer with following command\
<code>ssh root@boss.com:1008</code>\
Or use JuiceSSH from android phone to test

Accessing server with root password is not good idea.
- Create user *mytunnel* in boss.com
- replace root with *mytunnel* in above examples

Lastly do following to ensure that mytunnel user can do only tunneling work via boss.com
- in /etc/passwd in boss.com, replace shell of *mytunnel* from /bin/bash to /bin/false
- add *-f -N* in above commands
<code>ssh -f -N -R 1008:127.0.0.1:2048 root@boss.com</code>
<code>ssh -p2222 -f -N -R 1008:127.0.0.1:2048 root@boss.com</code>
- Now, user *mytunnel* can use only ssh-tunnel functinality from server
- 80 (apache),2048 (ssh) are ports of target
- 1008 is port of boss
- 2222 is port of boss where ssh-server is running.
- when root@boss.com is written , actually it is not calling boss.
- It only means root@target is accessed
- so target needs to know password of account at boss
- but client donot need to know password-at-boss
- but client need to know password-at-client
