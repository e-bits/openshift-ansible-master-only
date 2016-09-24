# OpenShift Origin Master Only Installation

This Repo will help you to setup a OpenShift Master Only Server based on CentOS. In this Repo you can find a host example file for [Ansible](https://www.ansible.com/) to provide a Master Only installation which is probably interesting for testing purposes. The setup process is based on [OpenShift Origin](https://docs.openshift.org/latest/install_config/install/advanced_install.html)

## Setup
### Description
In this example we are using a Windows machine with VirtualBox installed. Our **Controller** is a virtual machine on Windows with CentOS 7 installed. The Master is deticated server hardware.

### Download
Download [CentOS 7 as ISO](http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1511.iso) which will be used for controller and master installation

### Install Controller
  1. Install the Latest [VirtualBox](https://www.virtualbox.org/wiki/Downloads) version
  2. Setup a new virtual based on **RedHed x64** with standard settings
  3. Configure virtual machine for **bridged NIC** and about **2GB of Memory**
  4. Start the virtual machine and select the **CentOS 7 ISO** from above
  5. Set your Date and Time, Keyboard-Layout and Location. Do not forget to connect the networkadapter. Just set a password for root.
  6. Start the OS installation
  7. Take a look into **Configure and Update CentOS 7** step bellow
  8. From now on follow [Host Preparation](https://docs.openshift.org/latest/install_config/install/host_preparation.html)
    1. change Repo **epel-release-7-8.noarch.rpm**
      ```
      yum -y install \
  https://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-8.noarch.rpm
      ```
    2. Clone Git Repo with Submodules
      ```
      git clone --recursive  https://github.com/e-bits/openshift-ansible-master-only.git
      ```
    3. Copy `hosts.example` to `/etc/ansible/hosts` and change `hosts` to your needs
    4. Creating a ssh-key without password
      ```
      ssh-keygen
      ```
    5. Copy public key from controller to master
      ```
      ssh-copy-id -i ~/.ssh/id_rsa.pub 192.168.2.200
      ```

### Install Master
  1. SSH into Master using root
  2. Install Docker
    ```
    yum install -y docker    
    ```
    ```
    sed -i '/OPTIONS=.*/c\OPTIONS="--selinux-enabled --insecure-registry 172.30.0.0/16"' \
/etc/sysconfig/docker
    ```
  3. use an existing, specified volume group
  ```
  cat <<EOF > /etc/sysconfig/docker-storage-setup
VG=docker-vg
EOF
```

### Configure and Update CentOS 7
  1. Login to your newly created machine using root account.
  2. Get the actual ip address with `ip addr`
  3. Install pending updates `yum update -y`
  4. To edit files directly on CentOS 7 `yum install -y nano`
  5. Change hostname to controller or master. `nano /etc/hostname`
  6. `reboot`

- Install OpenShift via ansible
  1. check if your master is available
    ```
    ansible all -m ping
    ```
  2. start ansible playbook
    ```
    cd openshift-ansible-master-only/openshift-ansible
    ansible-playbook playbooks/byo/config.yml
    ```
  3. after some time OpenShift should be installed: `https://yourip:8443`
  4. to login to OpenShift you need to generate a user/password for htpasswd authentication or use your own method. The file is locatet at: `/etc/origin/master/htpasswd`. To generate a user/password in htpasswd you can use a service like this: [Htpasswd Generator](http://www.htaccesstools.com/htpasswd-generator/)
  5. login via ssh to your master and promote your created user from above to cluster-admin `oadm policy add-cluster-role-to-user cluster-admin root`

## Contributing
Please feel free to contribute to this repo and help improve my learning curve regarding OpenShift.
