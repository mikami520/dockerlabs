# How to setup 3 Node Kubernets Cluster on VMware vSphere ESXi VMs?

## Preparing the Infrastructure

- Machine #1: 100.98.26.206
- Machine #2: 100.98.26.210
- Machine #3: 100.98.26.213

## Assign hostname to each of these systems:

```
~$ cat /etc/hosts
127.0.0.1       localhost
127.0.1.1       ubuntu1804-1
100.98.26.206    kubemaster.dell.com
100.98.26.210   node1.dell.com
100.98.26.213   node2.dell.com
```

## Installing curl package

```
$ sudo apt install curl
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  libcurl4
The following NEW packages will be installed:
  curl libcurl4
0 upgraded, 2 newly installed, 0 to remove and 472 not upgraded.
Need to get 373 kB of archives.
After this operation, 1,036 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://us.archive.ubuntu.com/ubuntu bionic-updates/main amd64 libcurl4 amd64 7.58.0-2ubuntu3.7 [214 kB]
Get:2 http://us.archive.ubuntu.com/ubuntu bionic-updates/main amd64 curl amd64 7.58.0-2ubuntu3.7 [159 kB]
Fetched 373 kB in 2s (164 kB/s)
Selecting previously unselected package libcurl4:amd64.
(Reading database ... 128791 files and directories currently installed.)
Preparing to unpack .../libcurl4_7.58.0-2ubuntu3.7_amd64.deb ...
Unpacking libcurl4:amd64 (7.58.0-2ubuntu3.7) ...
Selecting previously unselected package curl.
Preparing to unpack .../curl_7.58.0-2ubuntu3.7_amd64.deb ...
Unpacking curl (7.58.0-2ubuntu3.7) ...
Setting up libcurl4:amd64 (7.58.0-2ubuntu3.7) ...
Processing triggers for libc-bin (2.27-3ubuntu1) ...
Processing triggers for man-db (2.8.3-2) ...
Setting up curl (7.58.0-2ubuntu3.7) ...
```

## Installing Docker

```
$ sudo curl -sSL https://get.docker.com/ | sh
# Executing docker install script, commit: 2f4ae48
+ sudo -E sh -c apt-get update -qq >/dev/null
+ sudo -E sh -c apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null
+ sudo -E sh -c curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" | apt-key add -qq - >/dev/null
Warning: apt-key output should not be parsed (stdout is not a terminal)
+ sudo -E sh -c echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" > /etc/apt/sources.list.d/docker.list
+ sudo -E sh -c apt-get update -qq >/dev/null
+ [ -n  ]
+ sudo -E sh -c apt-get install -y -qq --no-install-recommends docker-ce >/dev/null
+ sudo -E sh -c docker version
Client:
 Version:           18.09.7
 API version:       1.39
 Go version:        go1.10.8
 Git commit:        2d0083d
 Built:             Thu Jun 27 17:56:23 2019
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.7
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.8
  Git commit:       2d0083d
  Built:            Thu Jun 27 17:23:02 2019
  OS/Arch:          linux/amd64
  Experimental:     false
If you would like to use Docker as a non-root user, you should now consider
adding your user to the "docker" group with something like:

  sudo usermod -aG docker cse

Remember that you will have to log out and back in for this to take effect!

WARNING: Adding a user to the "docker" group will grant the ability to run
         containers which can be used to obtain root privileges on the
         docker host.
         Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface
         for more information.
cse@kubemaster:~$
```

```
~$ sudo docker version
Client:
 Version:           18.09.7
 API version:       1.39
 Go version:        go1.10.8
 Git commit:        2d0083d
 Built:             Thu Jun 27 17:56:23 2019
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.7
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.8
  Git commit:       2d0083d
  Built:            Thu Jun 27 17:23:02 2019
  OS/Arch:          linux/amd64
  Experimental:     false

```

## Add the Kubernetes signing key on both the nodes

```
$ sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
OK
```

## Adding Xenial Kubernetes Repository on both the nodes

```
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
```

## Installing Kubeadm

```
sudo apt install kubeadm
```

## Verifying Kubeadm installation

```
$ sudo kubeadm version
kubeadm version: &version.Info{Major:"1", Minor:"15", GitVersion:"v1.15.0", GitCommit:"e8462b5b5dc2584fdcd18e6bcfe9f1e4d970a529", GitTreeState:"clean", BuildDate:"2019-06-19T16:37:41Z", GoVersion:"go1.12.5", Compiler:"gc", Platform:"linux/amd64"}
```

##

