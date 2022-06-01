# Windows install



**EVERYONE**

Step 1: Enable WSL using Windows POWERSHELL:

{% hint style="success" %}
Launch POWERSHELL as administrator: search `powershell` from Windows search
{% endhint %}

```
Enable-WindowsOptionalFeature -Online -FeatureName $("VirtualMachinePlatform", "Microsoft-Windows-Subsystem-Linux")
```

Step 2: Install Debian using CMD (console):

{% hint style="success" %}
To open a CMD console search for `cmd` in the Windows search
{% endhint %}

```
wsl --set-default-version 2
wsl --install -d Debian
```

Step 3: Configure WSl to access the internet using LINUX (console):

{% hint style="success" %}
To get the Linux console, open a CMD console, type: `wsl`
{% endhint %}

```
sudo -s
rm -rf /etc/resolv.conf
bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
bash -c 'echo "[network]" > /etc/wsl.conf'
bash -c 'echo "generateResolvConf = false" >> /etc/wsl.conf'
chattr +i /etc/resolv.conf
exit
```

Step 4: install packages needed in WSL:

```
sudo apt-get update
sudo apt-get install gzip unzip vim git curl groff
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

Step 4 Troubleshooting:

* If you have accessing the internet from WSL first try RESTARTING the computer.
* If that does not work, try:

```
netsh winsock reset 
netsh int ip reset all
netsh winhttp reset proxy
ipconfig /flushdns
```

then restart the computer.

Step 5: Install Docker desktop for windows

Step 6: Install IDE:

* Microsoft Visual Studio Code (VSC)
* PHP Storm

Step 7: Install Dashlane password manager

****

****

**DEVELOPER**

* (linux console)
  * /usr/local/bin/aws --version (to check installed ok)
* add ssh keys to \~/.ssh
  * cd \~ && cp -r /mnt/c/Users/167926/.ssh \~/
* add aws creds to \~/.aws
* (linux console) Create the file /etc/fstab
  * insert the line c:/Users/167926/sources /home/david/sources drvfs default,metadata,uid=1000,gid=1000 0 0
  * sudo mount -a

**USEFUL**

* Restart the wsl service using POWERSHELL:&#x20;

```
Get-Service LxssManager | Restart-Service
```

* Ensure windows has profile/policies loaded properly using POWERSHELL:&#x20;

```
dmesg | grep 9p
```

* unmount the sources folder using LINUX console:

```
sudo umount /home/david/sources/
```

* remount the sources folder using LINUX console

```
sudo umount -a
```

* mount the sources folder manually using LINUX console&#x20;

```
sudo mount -t drvfs c:/Users/167926/sources /home/david/sources -o metadata,uid=1000,gid=1000
```

* Uninstall WSL:

```
wsl --disconnect
wsl --terminate -s debian
Disable-WindowsOptionalFeature -Online -FeatureName $("VirtualMachinePlatform", "Microsoft-Windows-Subsystem-Linux")

restart computer...
```



