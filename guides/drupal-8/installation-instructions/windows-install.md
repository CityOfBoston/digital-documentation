# Windows install

## **Basic WSL set up**

{% hint style="danger" %}
**Steps 1 - 7 must be completed while the computer is connected to the city network.**&#x20;
{% endhint %}

### **Step 1**: Enable WSL&#x20;

Using Windows POWERSHELL (as Administrator):

{% hint style="info" %}
Launch POWERSHELL as administrator: search `powershell` from Windows search
{% endhint %}

```
Enable-WindowsOptionalFeature -Online -FeatureName $("VirtualMachinePlatform", "Microsoft-Windows-Subsystem-Linux")
```

{% hint style="success" %}
**Alternative strategy**&#x20;

_This may work without Windows requesting a restart at the end._

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
Invoke-WebRequest -Uri https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi -OutFile $env:TMP\wsl_update_x64.msi -UseBasicParsing
msiexec.exe /i $env:TMP\wsl_update_x64.msi /passive
```
{% endhint %}

### **Step 2**: Install Debian&#x20;

Using CMD (console):

{% hint style="info" %}
To open a CMD console search for `cmd` in the Windows search
{% endhint %}

```
wsl --set-default-version 2
wsl --install -d Debian
```

{% hint style="success" %}
**Alternative strategy**:

_This may provide a more fault tolerant WSL environment when we are switching from City network to external network (because we are controlling where the distro is installed, and its not on the user's profile)._

```
mkdir %USERPROFILE%\WSLDistros
# Download the cobdistro.tar file into the users home folder (c:\Users\xxxx\)
wsl --set-default-version 2
wsl --import CoB-Debian %USERPROFILE%\WSLDistros %USERPROFILE%\cobdistro.tar
wsl --set-default CoB-Debian

# to create an image
# wsl --export CoB-Debian %USERPROFILE%\WSLDistros
```
{% endhint %}

### **Step 3**: Configure WSL to access the internet&#x20;

Using LINUX (WSL) console:

{% hint style="info" %}
To get the Linux console, open a CMD console, type: `wsl`
{% endhint %}

```
sudo -s
rm -rf /etc/resolv.conf
bash -c 'echo "nameserver 10.241.241.70" > /etc/resolv.conf'
bash -c 'echo "[network]" > /etc/wsl.conf'
bash -c 'echo "generateResolvConf = false" >> /etc/wsl.conf'
chattr +i /etc/resolv.conf
exit
```

### **Step 4: Custom WSL configurations**

@see [https://docs.microsoft.com/en-us/windows/wsl/wsl-config](https://docs.microsoft.com/en-us/windows/wsl/wsl-config)

These configuration files tweak the WSL environments to enable a better developer experience based on a standard CoB laptop configuration (i.e. minimum i7 chip, 32GB RAM and SSD harddisk).

Using a POWERSHELL console from the windows host:

```
echo '[wsl2]' > $env:USERPROFILE/.wslconfig
echo 'memory=16GB' >> $env:USERPROFILE/.wslconfig
```

Using a LINUX console (WSL):

```
cat << EOF > /etc/wsl.conf
[automount]
# Stops the entire hard disk(s) from being mounted into WSL
enabled = false
# Causes the /etc/fstab file to be processed as WSL starts
mountFsTab = true
options = "metadata,uid=1000,gid=1000
# Set whether WSL supports interop process like launching Windows apps and adding path variables. Setting these to false will block the launch of Windows processes and block adding $PATH environment variables.
[interop]
enabled = false
appendWindowsPath = false
# Network host settings that enable the DNS server used by WSL 2. This example changes the hostname, sets generateHosts to false, preventing WSL from the default behavior of auto-generating /etc/hosts, and sets generateResolvConf to false, preventing WSL from auto-generating /etc/resolv.conf, so that you can create your own (ie. nameserver 1.1.1.1).
[network]
hostname = DoITWSL
generateHosts = false
generateResolvConf = false
EOF

wsl --shutdown
[Wait 30 seconds]
wsl
```

### **Step 5**: install packages needed in WSL

Using LINUX console

```
sudo apt-get update
sudo apt-get install -y gzip unzip vim git curl groff

# optional for network diagnostics
sudo apt-get install -y net-tools dnsutils

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
sudo rm -f awscliv2.zip
```

#### **Troubleshooting**:

* If you have accessing the internet from WSL first try RESTARTING the computer.
* If that does not work, using a LINUX console try:

```
netsh winsock reset 
netsh int ip reset all
netsh winhttp reset proxy
ipconfig /flushdns
```

\=> then restart the computer.

### Step 6: Mount local folders into WSL.

Mount your development folders into WSL using the LINUX console:&#x20;

```
sudo -s
rm -rf /etc/fstab
echo "c:/Users/xxxx/sources /home/yyyy/sources drvfs default,metadata,uid=1000,gid=1000 0 0" > /etc/fstab
mkdir /home/yyyy/sources
mount -a
exit
```

{% hint style="info" %}
1. Replace `c:/Users/xxxx/sources` with the location in the windows host where you plan to keep all development source files. \
   _This is the folder where you will be cloning the CoB repos._ \
   _If in doubt, create a `sources` folder in your **windows home** folder, and for the command above just replace `xxxx` with your CoB supplied EmployeeID/User Account._
2. Replace `yyyy` with the accountname you used when you installed WSL (you can find this in the LINUX console by running `cd ~ && pwd` - the path displayed be in the format /home/accountname
{% endhint %}

### **Step 7**: Install Docker Desktop for Windows

1. Download installer from h[ttps://docs.docker.com/desktop/windows/install/](https://docs.docker.com/desktop/windows/install/)
2. Double click the installer to launch:\
   \+ Click OK to accept non-windows app,\
   \+ Select WSL2 as the backend (rather than Hyper-V)
3. Docker desktop does not automatically start after the install, you need to start it the first time from the Start menu.

{% hint style="warning" %}
**Restart your computer after this step.**

_If you do not, and subsequently restart the computer while off the city network, your installation will be broken, and you will have to remove Docker and WSL, and start over._

_(see "Docker Fails to Restart" notes below to fix broken/non-functional WSL installs)_
{% endhint %}

## **Setup other developer tooling**

### Add AWS-CLI credentials

Verify AWS is installed using LINUX console:

```
/usr/local/bin/aws --version
```

You should see an output something like:

`aws-cli/2.7.4 Python/3.9.11 Linux/5.10.102.1` .....

Obtain your secret access keys for AWS from the AWS administrator, and then create the AWS credentials file using the LINUX console:

```
echo "[cityofboston]" > ~/.aws/credentials
echo "aws_access_key_id = xxxxxx" >> ~/.aws/credentials
echo "aws_secret_access_key = xxxxx" >> ~/.aws/credentials
```

{% hint style="info" %}
Alternatively. you could also create and edit the credentials file using `vim` which is installed in the WSL instance (from step 5 above).
{% endhint %}

### Add SSH keys

Add your ssh keys to into your windows account (typically into a windows folder on you home drive) and then from a LINUX console:

```
mkdir ~/.ssh
cp -r /mnt/c/Users/xxxx/.ssh  ~/.ssh
sudo chmod -R 600 ~/.ssh/*
sudo chmod -R 644 ~/.ssh/*.pub
sudo chmod 700 ~/.ssh
```

{% hint style="info" %}
_Replace xxxx with your EmployeeID/User Account from CoB._
{% endhint %}

### Install IDE:

* Microsoft Visual Studio Code (VSC)
* PHP Storm

### Install Dashlane password manager

****

## **Knowledgebase**

#### Restart the WSL service on Windows host

Using POWERSHELL:&#x20;

```
Get-Service LxssManager | Restart-Service
```

#### Ensure windows has profile/policies loaded properly&#x20;

Using POWERSHELL:&#x20;

```
dmesg | grep 9p
```

#### Mount the sources folder manually&#x20;

Using LINUX console:

```
sudo mount -t drvfs c:/Users/xxxx/sources /home/yyyy/sources -o metadata,uid=1000,gid=1000
```

{% hint style="info" %}
&#x20;_Replace `xxxx` with your CoB supplied EmployeeID/User Account._

_Replace `yyyy` with the accountname you used when you installed WSL._
{% endhint %}

#### Un-mount the sources folder&#x20;

Using LINUX console:

```
sudo umount /home/yyyy/sources/
```

{% hint style="info" %}
_Replace `yyyy` with the accountname you used when you installed WSL._
{% endhint %}

#### Remount the sources folder&#x20;

Using LINUX console:

```
sudo umount -a
```

#### Uninstall WSL:

Using Powershell (as Administrator):

```
wsl --disconnect
wsl --terminate -s debian
dism.exe /online /disable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

#### Moving Home=>Office or Office=>Home

From Powershell console reinitialize WSL:

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

From LINUX (WSL) console reset the nameserver so you can access the internet:

```
bash -c 'echo "nameserver X.X.X.X" > /etc/resolv.conf'
```

Where X.X.X.X is the IPAddress: **8.8.8.8** _(confirm if there should be a different address)_ when in the office and **10.241.241.70** when not on the city network but using a VPN.

#### Docker Fails to start

If, when restarting the computer, Docker fails to start and/or you get the following error when starting WSL:

`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it.`

To fix this, perform the following steps.

**Step 1:** Using Powershell (ps) as Admin:

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

**Step 2:** Then using a CMD shell (as Admin)

```
sc config LxssManager start=auto
wsl --set-default-version 2
```

**Step 3:** Restart Docker for Windows from the start menu.
