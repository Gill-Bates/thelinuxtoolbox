# Setup SWAP

If you have a small machine, you will be happy about a little more RAM. This guide shows you how to set up 1 GB SWAP.


![GitHub Logo](/images/swap.png)

## Tutorial
### Create a SWAP-File on your Disk
```sudo fallocate -l 1G /var/spool/swap.file && sudo chmod 600 /var/spool/swap.file```
### Tell the OS that you wanna SAWP
```sudo mkswap /var/spool/swap.file && sudo swapon /var/spool/swap.file```
### Persist your setting for reboots
```sudo echo '/var/spool/swap.file none swap sw 0 0' | sudo tee -a /etc/fstab```
### Add a Memory rule, how the additional SWAP Space should be used
```sudo echo 'vm.swappiness=30' | sudo tee -a /etc/sysctl.conf && echo 'vm.vfs_cache_pressure=50' | sudo tee -a /etc/sysctl.conf```

Check the result with `top` or `htop`

## Delete SWAP (if neccessary)
### Tell the OS to turn off SWAP
```sudo swapoff /var/spool/swap.file```
### Remove the SWAP File from your system
```rm -r /var/spool/swap.file```
`reboot`
