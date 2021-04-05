# project description

this virtual machine realized with vagrant is an environment for siemens logo soft comfort 8.3 (slsc 8.3).

## what are the advantages of vagrant?
vagrant is a wrapper for hypervisor software. <br>
so you are able to dynamically provide a configured vm for a specific purpose.

## what is the purpose of this project?
slsc 8.3 can be a tough nut to crack when you want to install it. <br>
for this reason i wanted to provide a seperate environment which entails as few dependencies as possible.

# software requirements for slsc 8.3 ...

## ... from manual:
Linux: tested with SUSE Linux 12 SP1, Kernel 3.12.74. <br>
Software: Java Runtime Environment (JRE) 1.8.0_191. <br>
However, JRE will be delivered while Installation of slsc 8.3. <br>
<br>
source: <br>
https://cache.industry.siemens.com/dl/files/807/100782807/att_924631/v1/Help_de-DE_de-DE.pdf

## ... tried out:

Linux: <br>
openSUSE Leap 15.2, Kernel 5.3.18-lp152.66-default <br>
which is equivalent to ... <br>
SUSE Linux 15 SP2 <br>

source: <br>
https://de.wikipedia.org/wiki/SUSE_Linux_Enterprise_Server#Versionen <br>
https://app.vagrantup.com/opensuse/boxes/Leap-15.2.x86_64/versions/15.2.31.382

# software requirements for realized environment

## Virtualbox and VirtualBox Extension Pack:
https://www.virtualbox.org/wiki/Downloads <br>
sh ./00inst-vbox-extpack.sh <br>

## vagrant
https://www.vagrantup.com/downloads <br>

## copy siemens logo original cd content completely into...
mkdir -p ./install/cd/ <br>

## Download SLSC 8.3
https://support.industry.siemens.com/cs/document/109783154/download-f%C3%BCr-logo!-8-3-software-upgrade?dti=0&lc=de-WW <br>
https://support.industry.siemens.com/cs/attachments/109783154/lsc_v83_linux64.zip <br>
and put	it into	following folder: <br>
./install/lsc_v83_linux64.zip <br>

# Vagrantfile settings
See Vagrantfile. <br>

# build vm
sh ./01vagrant-up.sh <-- first boot: low screen resolution. maximize the gui window. <br>
vagrant halt <br>
sh ./01vagrant-up.sh <-- full screen resolution.
