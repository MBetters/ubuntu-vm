# My Ubuntu VM

## Prereqs

Download and add all these to your system `PATH`.

- [Packer](https://www.packer.io/downloads/)
- [Vagrant](https://www.vagrantup.com/downloads.html)
- [VirtualBox](https://www.virtualbox.org/)

## Build the Base VM

Change the `-var` flags to suit your needs. When you run the Packer command, you should see a VirtualBox GUI window running boot commands, then the login prompt while Packer tasks continue running in the terminal. Wait for the Packer command to finish running.

With Bash:

```bash
cd bento/packer_templates/ubuntu
packer build -only=virtualbox-iso -var "disk_size=65536" -var "cpus=4" -var "memory=4096" ubuntu-20.04-amd64.json
cd ../../..
```

Or, with PowerShell:

```powershell
cd .\bento\packer_templates\ubuntu
packer build -only=virtualbox-iso -var "disk_size=65536" -var "cpus=4" -var "memory=4096" ubuntu-20.04-amd64.json
cd ..\..\..
```

## Add the Base Box as a Vagrant Box

```bash
vagrant box add ubuntu_base bento/builds/ubuntu-20.04.virtualbox.box
```

## Build the Vagrant VM

```bash
vagrant up
```

## Verify the VM's Specs Are What You Set in Packer

```bash
# Start an SSH session in the VM
vagrant ssh
# Check the available memory (RAM)
free -m
# Check the available disk space
df -h
# Check the number of available CPUs
nproc
# Leave the SSH session
exit
```

## Setup SSH

If you use VS Code:

- Download the `Remote Development` extension
- Go to the `Remote Explorer` pane
- Hover over `SSH Targets` and click the cog button (`Configure`)
- Select the SSH config file you wish to edit.

If you don't use VS Code, just open your machine's `~/.ssh/config` in an editor.

Copy the output of this command into your SSH config file...

```bash
vagrant ssh-config
```

Except...

- Change `Host default` to `Host ubuntu-vm`.
- Delete the `UserKnownHostsFile /dev/null` line if you're on Windows.
