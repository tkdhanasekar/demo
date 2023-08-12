### fail2ban 

- Fail2Ban is then used to update firewall rules to reject the IP addresses for a specified amount of time
- Fail2ban is an intrusion prevention software framework
- Written in the Python programming language
- it is designed to prevent brute-force attacks.

Installation of fail2ban in centos

update the server
```
yum update -y
```
```
yum install epel-release -y
```
install the packages
```
yum install fail2ban fail2ban-systemd -y
```
copy the config file to local
```
cp -pf /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

ensure the parameters are set to below vaules
but bantime , findtime , maxretry are of your choice
```
vim /etc/fail2ban/jail.local
```
```
[DEFAULT]
ignoreip = 127.0.0.1/8 ::1         (92)
banaction = iptables-multiport
bantime = 120
findtime = 120
maxretry = 2
[sshd]
enabled = true          (288)

:wq! save and exit
```
add Jail file To protect SSH access
```
touch /etc/fail2ban/jail.d/sshd.local
chmod +x /etc/fail2ban/jail.d/sshd.local
```
```
vim /etc/fail2ban/jail.d/sshd.local
```
```
[sshd]
enabled = true
port = ssh
#action = firewallcmd-ipset
logpath = %(sshd_log)s
maxretry = 2
bantime = 120
:wq!
```
restart and enable firewalld
```
systemctl enable firewalld
systemctl start firewalld
```
enable and start fail2ban service
```
systemctl enable fail2ban
systemctl start fail2ban
```
To check for Fail2Ban Status
```
fail2ban-client status
```
```
iptables -L
```

Unbanning An IP Address immediately
```
fail2ban-client set sshd unbanip xx.xx.xx.xx
```
xx.xx.xx.xx is the ip address to be unbanned


