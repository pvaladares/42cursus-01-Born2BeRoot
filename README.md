# 42cursus-01-Born2beroot
This project aims to introduce you to the wonderful world of virtualization.


# Virtual Machine (VirtualBox)

1 - On the 42 machine, launch the [Managed Software Center](img/VM/1.png) app and search/install [VirtualBox 5.2.20](img/VM/2.png). To be noted that at the time of writing the latest version is 6.1 as can be checked [here](https://www.virtualbox.org/). To be noted that the alternative software `UTM` stated on the subject cannot be used since there are no admin privileges to install it nor it is available on the `Managed Software Center` to download/install. So `VirtualBox` is indeed the mandatory software to be used on this project.

2 - [Download](https://www.debian.org/download) the latest stable version of Debian and place it on `goinfre` folder (since it is less restrictive on size limits).

3 - Step by step instructions to create VM
* Launch `VirtualBox`
* [Select](img/VM/3.png) `New` and type `Born2beRoot` as name, choose `Linux` type and `Debian (64-bit)` version.
* Leave [memory size](img/VM/4.png) as default `1024 MB`

4 - If doing the **bonus**, since there is a limitation with the 5.2.20 version use the workaround described below:
> Note: The 5.2.20 version that can be used at 42 has a [limitation/bug](https://www.virtualbox.org/ticket/18177) with impact on the bonus, since it will not be possible to set a disk with size 30.8G
  * Go to terminal and type `VBoxManage createhd --filename Born2beRoot_DISK_31G54.vdi --size 31540 --format VDI`
  * Choose `Use an existing virtual hard disk file` and [select the newly created file](img/VM/5.png)
  * [Go to](img/VM/6.png) `Settings` and `Storage`, under `Storage Devices` choose `Empty` and then click on the "CD/Disk" icon on the right of the `Optical Drive` and select option `Choose Virtual Optical Disk File...`; [Select](img/VM/7.png) the debian ISO file that was privously saved on goinfre
 
 * Now just `Start` the VM to start the installation procedure of Debian.

# Debian Installation

## General
* [Select](img/Install/1.png) `Install` instead of any other option (remember, no graphical install is allowed)
* [Language](img/Install/2.png): select default `English`
* [Location](img/Install/3.png): select `other`, `Europe` and then `Portugal`
* [Local configuration](img/Install/4.png): select default `United States`
* [Keyboard](img/Install/5.png): `American English`
> The hostname of your virtual machine must be your login ending with 42 (e.g., wil42)
* Hostname: `pvaladar42`
* [Domain name](img/Install/7.png): *empty*
> You have to implement a strong password policy.
> 
> To set up a strong password policy, you have to comply with the following requirements:
> - (...)
> - Your password must be at least 10 characters long. It must contain an uppercase letter and a number. Also, it must not contain more than 3 consecutive identical characters.
> - The password must not include the name of the user.
* Root password: *see rules above*
> In addition to the root user, a user with your login as username has to be present.
* User account:
  * [Full name](img/Install/8.png): `Pedro Valadares`
  * [Username](img/Install/9.png): `pvaladar`
 * Password: *see rules above*
* Timezone: `Lisbon`

## Disk


# Resources
* [Born2beRoot Correction](https://github.com/sltcestloic/born2beroot_correction/blob/master/correction_born2beroot.pdf)
