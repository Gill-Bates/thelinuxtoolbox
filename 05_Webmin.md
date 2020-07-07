# Webmin
Webmin is a "All-in-One"-Tool to administrate your Linux-Distribution from a Webbrowser. It comes with a nice dashboard

## Tutorial ##
#### 1. Add Repository
sudo echo 'deb https://download.webmin.com/download/repository sarge contrib' | sudo tee -a /etc/apt/sources.list

#### 2. Add the key
```sudo apt install gnupg -y && wget http://www.webmin.com/jcameron-key.asc && apt-key add jcameron-key.asc```

#### 3. Install
```sudo apt update && apt install apt-transport-https webmin```

#### 4. Tune up
So far these instructions are standard up to this point. But now we want to secure the web interface against false logins with fail2ban.

Add

```nano /etc/fail2ban/jail.conf```

```
[webmin-auth]
enabled = true
maxretry = 3
```