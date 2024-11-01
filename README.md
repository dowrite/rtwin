# rtwin
RT Windows gives you a trial Windows 10 Enterprise VM with these additional customizations:

### Additional FOSS tools
  - chocolatey
  - 7zip
  - Eric Zimmerman Tools
  - immunity debugger

# INSTALLATION
### PREREQUISITES
The following SW must be installed on the host machine before following the installation steps: 
  - [Vagrant](https://developer.hashicorp.com/vagrant/downloads)
  - VMWare Workstation Pro or [VMWare Workstation Player](https://www.vmware.com/content/vmware/vmware-published-sites/us/products/workstation-player.html)
  - [VS Code](https://code.visualstudio.com/download)

### 1. Install Vagrant VMWare Utility
  - Download & Install the driver: https://developer.hashicorp.com/vagrant/downloads/vmware
  - Install the vagrant plugin `vagrant plugin install vagrant-vmware-desktop`

### 2. Clone this repo
  - Open VS Code
  - Click "Clone Repository"
  - Enter "https://github.com/dowrite/rtwin.git"

### 3. Create VM
  - Open Terminal in VS Code. Type the following commands in VS Code's terminal.
    ```
    cd rtwin
    vagrant up
    ```
  - While vagrant creates the VM for the first time, do `ctrl + f` in the terminal and do CASESENSITIVE search for ` E:` to highlight potential issues.
    
### 4. Troubleshoot VM Provisioning
The first time `vagrant up` is run, the VM is created and `vagrant provision` is automatically run. However, this step is most problematic since we're installing many tools. 
  - If provisioning fails/stalls
    - Reboot the VM and run `vagrant provision`, which re-runs the provisioning scripts
  - If errors continue, force a new download of the `gusztavvargadr/windows-10` box:
    ```
    vagrant box remove gusztavvargadr/windows-10
    vagrant up
    ``` 
### 5. Login
  - Login to rtwin (default creds: vagrant/vagrant)
