# Windows install



BUILD

* enable wsl
  * (powershell) Enable-WindowsOptionalFeature -Online -FeatureName $("VirtualMachinePlatform", "Microsoft-Windows-Subsystem-Linux")
* install Debian
  * (powershell) wsl --install -d Debian
* ensure wsl2 is being used
  * (powershell) wsl --set-default-version 2
*   (linux console)

    * Set up so swl can access the internet.

    ```
    sudo rm -rf /etc/resolv.conf
    sudo bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
    sudo bash -c 'echo "[network]" > /etc/wsl.conf'
    sudo bash -c 'echo "generateResolvConf = false" >> /etc/wsl.conf'
    sudo chattr +i /etc/resolv.conf
    ```

    * sudo apt-get update
    * If you have trouble try:

    ```
    netsh winsock reset 
    netsh int ip reset all
    netsh winhttp reset proxy
    ipconfig /flushdns
    ```

    * sudo apt-get install gzip unzip vim git curl groff
    * curl "https://awscli.amazonaws.com/awscli-exe-linux-x86\_64.zip" -o "awscliv2.zip"
    * unzip awscliv2.zip
    * sudo ./aws/install
* Docker desktop for windows
* Microsoft Visual Studio Code (VSC)
* PHP Storm
* Dashlane

DEVELOPER

* (linux console)
  * /usr/local/bin/aws --version (to check installed ok)
* add ssh keys to \~/.ssh
  * cd \~ && cp -r /mnt/c/Users/167926/.ssh \~/
* add aws creds to \~/.aws
* (linux console) Create the file /etc/fstab
  * insert the line c:/Users/167926/sources /home/david/sources drvfs default,metadata,uid=1000,gid=1000 0 0
  * sudo mount -a

USEFUL

* Restart the wsl service: (ps) Get-Service LxssManager | Restart-Service
* Ensure windows has profile/policies loaded properly (ps) dmesg | grep 9p
* unmount the sources folder (linux console) sudo umount /home/david/sources/
* remount the sources folder (linux console) sudo umount -a
* mount the sources folder manually (linux console) sudo mount -t drvfs c:/Users/167926/sources /home/david/sources -o metadata,uid=1000,gid=1000
