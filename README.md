# OpenShift Origin Master Only Installation

This Repo will help you to setup a OpenShift Master Only Server based on CentOS. In this Repo you can find a host example file for [Ansible](https://www.ansible.com/) to provide a Master Only installation which is probably interesting for testing purposes. The setup process is based on [OpenShift Origin](https://docs.openshift.org/latest/install_config/install/advanced_install.html)

## Setup
### Description
In this example we are using a Windows machine with VirtualBox installed. Our **Controller** is a virtual machine on Windows with CentOS 7 installed. The Master is deticated server hardware.

- Download [CentOS 7 as ISO](http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1511.iso) which will be used for controller and master installation

- Install Controller
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

- Configure and Update CentOS 7
  1. Login to your newly created machine using root account.
  2. Get the actual ip address with `ip addr`
  3. Install pending updates `yum update`
  4. To edit files directly on CentOS 7 `yum install nano`
  5. Change hostname to controller or master. `nano /etc/hostname`
  6. `reboot`

## Contributing
Please feel free to contribute to this repo and help improve my learning curve regarding OpenShift.
