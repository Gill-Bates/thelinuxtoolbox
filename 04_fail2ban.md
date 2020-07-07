# Install & Setup Fail2ban
Fail2ban is extremely easy to install ```apt install fail2ban```. There are numerous instructions on the net. Why another one? Very simple: Because every manual on the net ***was always missing some detail.*** These instructions not only show you how to install fail2ban, but also how to make it really good!

![GitHub Logo](/images/fail2ban.png)

## Please note
* fail2ban is installed inside `/etc/fail2ban`
* fail2ban's configuration is inside a `jail.local` file (located inside inside `/etc/fail2ban`). We have to create this file
* Every change on your `jail.local` requires a reload of the Service `systemctl reload fail2ban`

## Setup
### Create the `jail.local`
***Replace `<yourpublicip>` with your own IP. Don't remove the `/32` at the end!***

Open the editor with `nano /etc/fail2ban/jail.local`. Copy & Paste the following code:
```
### The Baseline
Copy & Paste this code
`###### Configuration ######
[DEFAULT]
ignorself = true
# Never block these IPs:
ignoreip = <yourpublicip>/32
# The number of failures before a host get banned
maxretry = 5
# Ban Client with ufw
banaction = ufw

# SSH
[sshd]
enabled  = true
filter = sshd
badips[category=ssh]
bantime  = 48h
maxretry = 3
findtime = 10m
```
Save & Close

Secure your file with `chmod 640 /etc/fail2ban/jail.local`
