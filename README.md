# Description
Documentation install k3s under Windows 10

## Dependencies

- Windows 10 Home or later

## Install Kernel WSL Application (Ubuntu 20.04)
```
wsl --install -d Ubuntu
```

## Set Ubuntu App default app if not
```
wsl -s Ubuntu
```

## install some dependencies
```
sudo apt-get update
sudo apt-get install wget
sudo apt-get install curl
```

## Add genie repository
```
sudo su
wget -O /etc/apt/trusted.gpg.d/wsl-transdebian.gpg https://arkane-systems.github.io/wsl-transdebian/apt/wsl-transdebian.gpg

chmod a+r /etc/apt/trusted.gpg.d/wsl-transdebian.gpg

cat << EOF > /etc/apt/sources.list.d/wsl-transdebian.list
deb https://arkane-systems.github.io/wsl-transdebian/apt/ $(lsb_release -cs) main
deb-src https://arkane-systems.github.io/wsl-transdebian/apt/ $(lsb_release -cs) main
EOF

apt update

exit
```

## Install systemd genie
```
sudo apt install -y systemd-genie
```

## Remove systemd errors

Check mount device
```
mount
```

Remove **systemd-remount-fs.service** error check device and execute the next command with the device checked previusly
```
sudo e2label /dev/sdb cloudimg-rootfs
```

Remove **ssh.service** error
```
sudo ssh-keygen -A
```

Remove **multipathd.service** error
```
sudo systemctl disable multipathd.service
```

## Install k3s by default with only one node
```
curl -sfL https://get.k3s.io | sh -

[INFO]  Finding release for channel stable
[INFO]  Using v1.23.8+k3s2 as release
[INFO]  Downloading hash https://github.com/k3s-io/k3s/releases/download/v1.23.8+k3s2/sha256sum-amd64.txt
[INFO]  Downloading binary https://github.com/k3s-io/k3s/releases/download/v1.23.8+k3s2/k3s
[INFO]  Verifying binary download
[INFO]  Installing k3s to /usr/local/bin/k3s
[INFO]  Skipping installation of SELinux RPM
[INFO]  Creating /usr/local/bin/kubectl symlink to k3s
[INFO]  Creating /usr/local/bin/crictl symlink to k3s
[INFO]  Creating /usr/local/bin/ctr symlink to k3s
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s.service
[INFO]  systemd: Enabling k3s unit
Created symlink /etc/systemd/system/multi-user.target.wants/k3s.service -> /etc/systemd/system/k3s.service.
[INFO]  systemd: Starting k3s
```

## Configure k3s to use kubectl CLI directly

```
mkdir .kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $USER ~/.kube/config
chmod 600 ~/.kube/config
```

Copy this line inside .bashrc
```
export KUBECONFIG=~/.kube/config
```

## Check Kubernetes
```
kubectl cluster-info
kubectl get nodes
```

## Start WSL
```
wsl genie -s
```

## Stop WSL
```
wsl
```

## Some links
- WSL Genie: https://github.com/arkane-systems/genie
- WSL Genie systemd errors:https://github.com/arkane-systems/genie/wiki/Systemd-units-known-to-be-problematic-under-WSL
- k3s Site: https://k3s.io/
