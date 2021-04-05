Vagrant.configure("2") do |config|
   config.vm.box = "opensuse/Leap-15.2.x86_64"
   config.vm.box_version = "15.2.31.382"
config.vm.provider "virtualbox" do |v|
   v.name = "suse-logo"
   v.gui = true
   v.memory = 4096
   v.cpus = 2
   v.customize ["modifyvm", :id, "--vram", "32"]
   v.customize ["modifyvm", :id, "--ioapic", "on"]
   v.customize ["modifyvm", :id, "--uart1", "0x3f8", "4"]
   v.customize ["modifyvm", :id, "--uartmode1", "server", "/tmp/my_tty"]
#   v.customize ["setextradata", "global", "GUI/MaxGuestResolution", "auto"]
#   v.customize ["setextradata", "global", "GUI/MaxGuestResolution", "any"]
   v.customize ["setextradata", :id, "CustomVideoMode1", "1920x1080x32"]
   v.customize ['setextradata', :id, 'GUI/LastGuestSizeHint','1920,1080']
   v.customize 'post-boot', ["controlvm", :id, "setvideomodehint", "1920", "1080", "32"]
   v.customize ['setextradata', :id, 'GUI/ScaleFactor', '2.00']
   v.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
   v.customize ['sharedfolder', 'add', :id, '--name', 'install', '--hostpath', './install']
   v.customize ['sharedfolder', 'add', :id, '--name', 'documents', '--hostpath', './documents']
end

config.vm.provision "shell", inline: <<-SHELL

# VERR_ACCESS_DENIED when trying to add a raw disk to virtual box
# https://unix.stackexchange.com/questions/284704/verr-access-denied-when-trying-to-add-a-raw-disk-to-virtual-box/490344
usermod -a -G disk $USER

# https://askubuntu.com/questions/252853/how-to-mount-a-virtualbox-shared-folder-at-startup

mkdir -p /home/vagrant/Downloads/install
mkdir -p /home/vagrant/Documents/shared
echo "install     /home/vagrant/Downloads/install  vboxsf  auto,rw,comment=systemd.automount,uid=1000,gid=1000 0 0" >> /etc/fstab
echo "documents   /home/vagrant/Documents/shared  vboxsf  auto,rw,comment=systemd.automount,uid=1000,gid=1000 0 0" >> /etc/fstab
mount -a

# install libs for siemens logo
zypper --non-interactive install hostname 
#glibc-32bit libXtst6-32bit xorg-x11-libs

# install display manager
zypper --non-interactive install sddm
sed -i -e "s/DISPLAYMANAGER=\"default\"/DISPLAYMANAGER=\"sddm\"/" /etc/sysconfig/displaymanager
rm /etc/systemd/system/default.target
ln -fs /usr/lib/systemd/system/graphical.target /etc/systemd/system/default.target
# install desktop
zypper --non-interactive install -t pattern kde kde_plasma

# install additional tools
zypper --non-interactive install konsole kwrite dolphin okular unzip

# change keymap to german
echo "KEYMAP=de-latin1-nodeadkeys" > /etc/vconsole.conf

mkdir -p /home/vagrant/Desktop/

cat <<EOF > /home/vagrant/Desktop/install-logo.desktop
[Desktop Entry]
Comment[en_US]=
Comment=
Exec=/home/vagrant/Downloads/install/lsc_v83_linux64-install.sh
GenericName[en_US]=
GenericName=
Icon=install
MimeType=
Name[en_US]=install-logo
Name=install-logo
Path=/home/vagrant/Downloads/install
StartupNotify=true
Terminal=false
TerminalOptions=
Type=Application
X-DBUS-ServiceName=
X-DBUS-StartupType=
X-KDE-SubstituteUID=false
X-KDE-Username=
EOF

cp /home/vagrant/Downloads/install/host-data-exchange.pdf /home/vagrant/Desktop/host-data-exchange.pdf
cp /home/vagrant/Downloads/install/install-manual.pdf /home/vagrant/Desktop/install-manual.pdf 

mkdir -p /home/vagrant/bin/Siemens/LOGOComfort_V8.3

chown -R vagrant:vagrant /home/vagrant

reboot
SHELL

end
