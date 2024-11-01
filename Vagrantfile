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
    python -m pip install FakeDNS
      # Set up a script to run FakeDNS
    $scriptPath = "C:\\FakeDNS"
    New-Item -ItemType Directory -Force -Path $scriptPath
    Set-Content -Path "$scriptPath\\run_fakedns.ps1" -Value 'python -m FakeDNS -p 53 -a 127.0.0.1'

    # Download and install Immunity Debugger
    $immunityUrl = "https://www.immunityinc.com/downloads/debugger/ImmunityDebuggerSetup.exe"
    $installerPath = "$env:TEMP\\ImmunityDebuggerSetup.exe"
    Invoke-WebRequest -Uri $immunityUrl -OutFile $installerPath
    Start-Process -FilePath $installerPath -ArgumentList "/S" -NoNewWindow -Wait
    Remove-Item -Path $installerPath -Force


  SHELL
end
