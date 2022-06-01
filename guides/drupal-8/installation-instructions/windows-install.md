# Windows install



**EVERYONE**

{% hint style="danger" %}
**Steps 1 - 5 must be completed while the computer is connected to the city network.**&#x20;
{% endhint %}

**Step 1**: Enable WSL using Windows POWERSHELL:

{% hint style="success" %}
Launch POWERSHELL as administrator: search `powershell` from Windows search
{% endhint %}

```
Enable-WindowsOptionalFeature -Online -FeatureName $("VirtualMachinePlatform", "Microsoft-Windows-Subsystem-Linux")
```

**Step 2**: Install Debian using CMD (console):

{% hint style="success" %}
To open a CMD console search for `cmd` in the Windows search
{% endhint %}

```
wsl --set-default-version 2
wsl --install -d Debian
```

**Step 3**: Configure WSl to access the internet using LINUX (console):

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

**Step 4**: install packages needed in WSL:

```
sudo apt-get update
sudo apt-get install -y gzip unzip vim git curl groff
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

**Troubleshooting:** Step 4:

* If you have accessing the internet from WSL first try RESTARTING the computer.
* If that does not work, try:

```
netsh winsock reset 
netsh int ip reset all
netsh winhttp reset proxy
ipconfig /flushdns
```

then restart the computer.

**Step 5**: Install Docker desktop for windows

1. Download installer from h[ttps://docs.docker.com/desktop/windows/install/](https://docs.docker.com/desktop/windows/install/)
2. Double click the installer to launch:\
   \+ Click OK to accept non-windows app,\
   \+ Select WSL2 as the backend (rather than Hyper-V)
3. Docker desktop does not automatically start after the install, you need to start it the first time from the Start menu.

{% hint style="info" %}
**Restart your computer after this step.**

_If you do not, and subsequently restart the computer while off the city network, your installation will be broken, and you will have to remove Docker and WSL, and start over._
{% endhint %}

**Step 6:** Install IDE:

* Microsoft Visual Studio Code (VSC)
* PHP Storm

**Step 7**: Install Dashlane password manager

****

****

**DEVELOPER**

* Verify AWS is installed using LINUX console:

```
/usr/local/bin/aws --version
```

* Add your ssh keys to into your windows account (typically into a windows folder on you home drive) and then from a LINUX console:

```
ln -s  /mnt/c/Users/XXX/.ssh ~/.ssh
```

{% hint style="info" %}
Replace XXX with your EmployeeID/User Account from CoB.
{% endhint %}

* Obtain your secret access keys for AWS from the AWS administrator, and then create the AWS credentials file using the LINUX console:

```
echo "[cityofboston]" > ~/.aws/credentials
echo "aws_access_key_id = xxxxxx" >> ~/.aws/credentials
echo "aws_secret_access_key = xxxxx" >> ~/.aws/credentials
```

{% hint style="info" %}
You could also create and edit the credentials file using `vim` which is installed in the WSL instance (from step 4 above).
{% endhint %}

* Mount your development folders into WSL using the LINUX console:&#x20;

```
sudo -s
rm -rf /etc/fstab
echo "c:/Users/xxxx/sources /home/yyyy/sources drvfs default,metadata,uid=1000,gid=1000 0 0" > /etc/fstab
mkdir /home/yyyy/sources
mount -a
exit
```

{% hint style="success" %}
1. Replace `c:/Users/xxxx/sources` with the location in the windows host where you plan to keep all development source files. \
   _This is the folder where you will be cloning the CoB repos._ \
   _If in doubt, create a `sources` folder in your **windows home** folder, and for the command above just replace `xxxx` with your CoB supplied EmployeeID/User Account._
2. Replace `yyyy` with the accountname you used when you installed WSL (you can find this in the LINUX console by running `cd ~ && pwd` - the path displayed be in the format /home/accountname
{% endhint %}

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



