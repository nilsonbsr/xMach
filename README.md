<!---# mach4
Hier ist eine Sammlung von MACH4 Dokumentation 

**Einführung**

Das Booten eines Mikro-Kernel/Server-Systems ist etwas komplizierter als das Booten eines Makro-Kernelsystems, da mehr Dateien vorhanden sein müssen und korrekt funktionieren müssen, bevor das System Eingaben des Benutzers akzeptiert.

Die CMU-Umgebung hat auch zu einer gewissen **Komplexität** des üblichen BSD-Unix-Setups beigetragen

1 - In Mach 3.0 werden folgende Teile für einen erfolgreichen Start benötigt: 

- den 16-Sektor-Boot-Code (in der Regel vom Hersteller des Rechners geliefert), den Mikrokernel
- eine Auslagerungsdatei(paging file)
- einen Server, den der Kernel aufruft
- eine Emulationsbibliothek (zumindest für unsere Server)
- ein Benutzerprogramm zum Starten.

2-   Die anderen Komplikationen sind die Tatsache, dass wir normalerweise ein Super-Root und ein lokales Root haben, um das CMU-RFS-Dateisystem zu unterstützen, und die Tatsache, dass man beim Hochfahren als Einzelbenutzer keinen Root-Zugang hat, sondern mit der Benutzerkennung "opr" läuft.

3-  Außerdem haben wir auf CMU-Rechnern eine /etc/rc, die darauf besteht, dass /vmunix ein symbolischer Link zu dem Unix-Server ist, auf dem Sie laufen, bevor der Bootvorgang zum Multi-User abgeschlossen wird.

-->

## Introduction

Booting a micro-kernel/server system is a bit more complicated than booting a macro-kernel system because more files must be present and functioning correctly before the system will accept input from the user. Our goal is to provide a step-by-step guideline to install a _**xMach**_ microkernel on a modern computer. This documentation is based on our trial and error steps and is an ongoing project.

_a special thanks to [**neozeed**](https://virtuallyfun.com/) for providing us all the resources_.


___

### _Best practice_
We encountered some problems during the installation of _xMach_ under WSL, which were not the case under native _**Ubuntu**_. So, we recommend starting with Ubuntu to save time and nerves
___

## Prerequisites

### _Dependencies you might need to install in advance_
* [bison](https://www.gnu.org/software/bison/)
* [flex](https://geeksww.com/tutorials/operating_systems/linux/installation/installing_flex_fast_lexical_analyzer_ubuntu_linux.php)
* [gawk](https://phoenixnap.com/kb/gawk-linux)

<!--all needed matrial can be found [here](https://github.com/neozeed/xMach/releases/tag/v1_0).
-->
After our first two approaches, including [installation of  _**Mach 4**_ from the University of Utah](https://www-old.cs.utah.edu/flux/mach4/html/) under KVM, were unsuccessful, we decided to give windows 10 a try. 

### _Installing WSL2_ 
First, we need to install _**WSL2**_ as well as a _**Debian-based Linux**_, preferably Ubuntu 20.4.x. To do that, open either Windows Command Prompt or PowerShell as an administrator and type the command below. 

```
wsl --install
```

After that, install Ubuntu from the Microsoft Store.

### _Setup 32-bit Environment_

Now let's set up a 32-bit development enviroment on our 64-bit Debian-based Linux system, ensuring that necessary libreries and tools for compiling and running 32-bit applications. 

```
dpkg --add-architecture i386
apt-get update
apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386 libc6-dev:i386 libfl-dev:i386
apt-get install sharutils gcc-multilib build-essential
```

### _Adding "GCC 2.7.2.3" into PATH_  

Next step is to add old _**GCC 2.7.2.3**__ into our linux PATH-Environment and make sure to do this step first. For some reason this affects the whole process if it's not done at the beginning. To do that:

```
export PATH=/usr/local/i586-linux2/bin:/usr/local/i586-linux2/lib/gcc-lib/i586-linux/2.7.2.3:$PATH
```
make sure to also copy/ move the whole content of _**i586-linux2.tar.gz**_ to _/usr/local/_.
### _Source-Code & Permissions_
one more step, and then we are all set to start compiling. Extract **_xMach-1_0_** and make sure that all the sub-folders have executable permissions by simply execute the following codes.

```
chmod +x kernel/c*
chmod +x kernel/*sh
chmod +x lites/conf*
chmod +x lites/conf/*
```

### _Building the xMach Kernel_

First, we have to create a folder named _**'kobject'**_ in our cloned repository. Afterwards, change to this directory and execute the command below.

```
../kernel/configure --host=i586-linux --target=i586-linux --build=i586-linux --enable-elf --enable-libmach --enable-linuxdev --prefix=/usr/local/xmach
```

According to original Documentation from [**neozeed**](https://virtuallyfun.com/) 
 > configure never populates the compiler tools right. 

To solve this problem, we have to copy the _**'Makeconf'**_ file from this path (/updated-conf/kernel/Makeconf), and then we are good to go. 

```
cp ../updated-conf/kernel/Makeconf .
```
Type _**make**_ and wait for all steps to run through. 