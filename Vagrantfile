# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "gusztavvargadr/windows-10"

  # VMWare-specific configuration  
  config.vm.provider :vmware_desktop do |vmware|
    vmware.gui = true
    # Customize the amount of memory on the VM:
    vmware.memory = "8192"
    vmware.vmx['displayname'] = "RTWin"
    vmware.vmx["virtualHW.version"] = "16"
  end

  # Provisioning script to install Chocolatey and 7-Zip
  config.vm.provision "shell", inline: <<-SHELL
    # Install Chocolatey if not already installed
    if (-not (Get-Command choco -ErrorAction SilentlyContinue)) {
      Set-ExecutionPolicy Bypass -Scope Process -Force;
      [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072;
      iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
    }

    # Disable sleep
    powercfg -change -standby-timeout-ac 0
    powercfg -change -standby-timeout-dc 0

    # Disable screen lock
    Set-ItemProperty -Path "HKCU:\\Control Panel\\Desktop" -Name "ScreenSaveActive" -Value "0"
    Set-ItemProperty -Path "HKCU:\\Control Panel\\Desktop" -Name "ScreenSaveTimeOut" -Value "0"

    
    # Install 7-Zip
    choco install 7zip -y

    # Install Python 3 & pip
    choco install python -y
    $env:Path += ";$((Get-Command python).Path | Split-Path)"
    python -m ensurepip --upgrade
    python -m pip install --upgrade pip

    # Install Eric Zimmerman's tools
    choco install ericzimmermantools -y

    # Install FakeDNS
    python -m pip install fakedns
    fakedns-config init

    # Install VS Community with .NET
    if (!(Test-Path -Path "C:\\temp")) {
      New-Item -ItemType Directory -Path "C:\\temp"
    }
    Invoke-WebRequest -Uri "https://aka.ms/vs/17/release/vs_community.exe" -OutFile "C:\\temp\\vs_community.exe"
    Start-Process -FilePath "C:\\temp\\vs_community.exe" -ArgumentList '--quiet --wait --norestart --add Microsoft.VisualStudio.Workload.ManagedDesktop' -NoNewWindow -Wait

  SHELL
end
