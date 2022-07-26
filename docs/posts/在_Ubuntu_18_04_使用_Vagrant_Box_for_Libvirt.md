---
title: 在 Ubuntu 18.04 使用 Vagrant Box for Libvirt 建置 VM
date: 2018-10-20 11:21:00
description: 在 Ubuntu 18.04 透過 Vagrant Box for Libvirt ，快速建置 VM 的秘訣。 
sidebar: auto
tags:
    - Ubuntu
    - VM
    - Vagrant
---

在 Ubuntu 作業系統，可使用 virt-manager 管理符合 Libvirt API 規格之「虛擬機器（VM：Virtual Machine）」。

VM 之建置作業，採用 Vagrant Box 建置 VM 可降低不少時間。但 Vagrant Box 預設之 VM 執行環境為：VirtualBox。 若不熟悉這些「眉角」，每當執行 **vagrant up** 指令後，總會在終端機看到 VM 無法正常啟動的錯誤訊息。

本文件用於指引：在 Ubuntu 18.04 作業系統，如何正確安裝 Vagrant Box for Libvirt ；及正常啟動 VM。


## 安裝 Vagrant 軟體套件

以下為：Vagrant 2.2.6 安裝作業。

作業之程序步驟為：

- 先在 Client 端電腦（作業系統：macOS），透過 Web Brower 下載安裝檔案；
- 再於 Server 端電腦（作業系統：Ubuntu 18.04）進行 Vagrant 安裝。


### （1）在 Client 端下載安裝檔案

在 Mac 電腦，啟動 Web Browser 後，至 Vagrant 官網下載安裝檔案（.deb）。

> **https://www.vagrantup.com/downloads.html**


### （2）在 Server 端進行安裝

在 Client 端電腦啟動「終端機」，透過 ssh 登入伺服器。然後將先前已下載之 Vagrant 安裝檔案，複製到 Server 端的硬碟。 


    $ ssh web_admin@192.168.66.10
    $ cd ~/_tmp
    $ scp alanjui@192.168.66.100:~/Downloads/vagrant_2.2.6_x86_64.deb .


### （3）透過 dpkg 安裝 Vagrant

使用 dpkg 執行檔案格式為 .deb 之安裝作業。


    $ sudo dpkg -i vagrant_2.2.6_x86_64.deb
    選取了原先未選的套件 vagrant。
    （讀取資料庫 ... 目前共安裝了 359404 個檔案和目錄。）
    準備解開 vagrant_2.2.6_x86_64.deb ...
    解開 vagrant (1:2.2.6) 中...
    設定 vagrant (1:2.2.6) ...


### （4）驗證安裝結果

執行以下指令，確認 Vagrant 已完成安裝，且可正常運作。


    $ vagrant --version
    Vagrant 2.2.6



## 安裝 Vagrant Box for Libvirt

以下安裝之 Vagrant Box for Libvirt 為 Ubuntu 18.04.3 Desktop 版本。

```shell
$ vagrant box add peru/ubuntu-18.04-desktop-amd64 --provider=libvirt

$ mkdir -p ~/workspace/vagrant-libvirt/ubuntu-1804 && cd $_

$ vagrant init peru/ubuntu-18.04-desktop-amd64 --box-version 20190901.01

$ vagrant up
Bringing machine 'default' up with 'libvirt' provider...
==> default: Checking if box 'peru/ubuntu-18.04-desktop-amd64' is up to date...
==> default: Uploading base box image as volume into libvirt storage...
==> default: Creating image (snapshot of base box volume).
==> default: Creating domain with the following settings...
==> default:  -- Name:              ubuntu-1804_default
==> default:  -- Domain type:       kvm
==> default:  -- Cpus:              1
==> default:  -- Feature:           acpi
==> default:  -- Feature:           apic
==> default:  -- Feature:           pae
==> default:  -- Memory:            2048M
==> default:  -- Management MAC:
==> default:  -- Loader:
==> default:  -- Nvram:
==> default:  -- Base box:          peru/ubuntu-18.04-desktop-amd64
==> default:  -- Storage pool:      default
==> default:  -- Image:             /var/lib/libvirt/images/ubuntu-1804_default.img (50G)
==> default:  -- Volume Cache:      default
==> default:  -- Kernel:
==> default:  -- Initrd:
==> default:  -- Graphics Type:     spice
==> default:  -- Graphics Port:     -1
==> default:  -- Graphics IP:       127.0.0.1
==> default:  -- Base box:          peru/ubuntu-18.04-desktop-amd64
==> default:  -- Storage pool:      default
==> default:  -- Image:             /var/lib/libvirt/images/ubuntu-1804_default.img (50G)
==> default:  -- Volume Cache:      default
==> default:  -- Kernel:
==> default:  -- Initrd:
==> default:  -- Graphics Type:     spice
==> default:  -- Graphics Port:     -1
==> default:  -- Graphics IP:       127.0.0.1
==> default:  -- Graphics Password: Not defined
==> default:  -- Video Type:        qxl
==> default:  -- Video VRAM:        9216
==> default:  -- Sound Type:    ich6
==> default:  -- Keymap:            en-us
==> default:  -- TPM Path:
==> default:  -- INPUT:             type=mouse, bus=ps2
==> default:  -- CHANNEL:             type=unix, mode=
==> default:  -- CHANNEL:             target_type=virtio, target_name=org.qemu.guest_agent.0
==> default:  -- CHANNEL:             type=spicevmc, mode=
==> default:  -- CHANNEL:             target_type=virtio, target_name=com.redhat.spice.0
==> default:  -- RNG device model:  random
==> default: Creating shared folders metadata...
==> default: Starting domain.
==> default: Waiting for domain to get an IP address...
==> default: Waiting for SSH to become available...
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Configuring and enabling network interfaces...

$ vagrant status
Current machine states:

default                   running (libvirt)

The Libvirt domain is running. To stop this machine, you can run
`vagrant halt`. To destroy the machine, you can run `vagrant destroy`. 

$ vagrant ssh
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-58-generic x86_64)

    * Documentation:  https://help.ubuntu.com
    * Management:     https://landscape.canonical.com
    * Support:        https://ubuntu.com/advantage


    * Canonical Livepatch is available for installation.
    - Reduce system reboots and improve kernel security. Activate at:
        https://ubuntu.com/livepatch

0 packages can be updated.
0 updates are security updates.
```


## 參考文件

- [**How to Install and Configure KVM on Ubuntu 18.04 LTS Server**](https://www.linuxtechi.com/install-configure-kvm-ubuntu-18-04-server/)

- [**Vagrant and Libvirt with KVM or QEMU**](https://docs.cumulusnetworks.com/cumulus-vx/Development-Environments/Vagrant-and-Libvirt-with-KVM-or-QEMU/)

- [**How to Use Vagrant with Libvirt on Linux**](https://computingforgeeks.com/using-vagrant-with-libvirt-on-linux/)



