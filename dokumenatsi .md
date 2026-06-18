
nvim /etc/fstab

echo "user1 ALL=(ALL:ALL) ALL" > /etc/sudoers.d/user1

mkinitcpio -P

pacman -S networkmanager network-manager-applet

sudo systemctl stop iwd

## SERVER

sudo systemctl status sshd - harus aktif

sudo systemctl enable --now sshd

## ADMIN

ssh namauser/ipserver

sudo systemctl start sshd - jika tidak bisa: sudo systemctl stop firewalld

example: ssh rakha@192.168.2.145

sudo firewalld-cmd --list-all-zone | bersihin zone zone di firewalld

sudo firewalld-cmd --zone=dmz --remove-service=ssh --permanent

sudo firewalld-cmd --zone=eksternal --remove-service=ssh --permanent

sudo firewalld-cmd --zone=home --remove-service={dhcpv6-client, mdns, samba-client, ssh} --permanent

sudo firewalld-cmd --zone=internal --remove-service={dhcpv6-client, mdns, samba-client, ssh} --permanent

sudo firewalld-cmd --zone=public --remove-service=dhcpv6-client --permanent

sudo firewalld-cmd --zone=work --remove-service={dhcpv6-client, ssh} --permanent

sudo firewalld-cmd --reload

sudo firewalld 

sudo lsmod | grep cramfs *tidak keluar apa-apa= kosong*

sudo lsmod | grep freevxfs

sudo lsmod | grep hfsplus

sudo lsmod | grep jffs2

sudo lsmod | grep overlayfs

sudo lsmod | grep squashfs

sudo lsmod | grep udf

sudo lsmod | grep usb-storage

sudo nvim /etc/modprobe.d/disable-module.conf

ISI:

install usb-storage /bin/false

blacklist usb-storage

sudo mkinitcpio -P

# TAHAP SELANJUTNYA

## Set Up Podman

sudo nvim /etc/subuid

sudo nvim /etc/subgid

sudo systemctl enable --global podman

mkdir -p .config/containers *folder kontainer*

nvim .conf/containers/storage.conf

*Ketik:*

[storage]

driver = "overlay"

[storage.options.overlay]

mount_program = ""

mountopt = "userxattr"

sudo nvim /etc/containers/registries.conf

*Cari: #unqualified-search-registries = ["docker.io"]

## INSTALL SLIMS

cd .config/containers

sudo pacman -s podman-compose

wget -c http://github.com/slims/docker-compose-for-slims/archive/master.zip

sudo -paacman and.zip  

ls

sudo nvim /etc/sysctl.d/99-custome.conf

net.ipv4.ip_unprivileged_port=80

sudo sysctl --system

cd compose

ls

mv docker-compose.yaml docker-compose.yaml.bck

ls

nvim docker-compose.yaml

*Colok kabel lan*

## ADMIN

sudo usermod -aG wheel user1/user2

nmcli device status

ip a

nmcli connection add type ethernet ifname enp2s0 con-name "admin-connection" ipv4.method manual ipv4.adresses 212.122.4.6/24 ipv4.gateway 212.122.4.1 ipv4.dns 8.8.8.8 

echo "nmcli connection up admin-connection" >> /home/user1/.bash_profile

*logout*

nmcli connection add type ethernet ifnaamme enp2s0 con-name "operator-connection" ipv4.method manual ipv4.addresses 10.20.30.56/24 ipv4 gateway 10.20.30.1

echo "nmcli connection up operator-connection" >> /home/operator/.bash_profile

*logout*

ssh rakha@192.168.2.145

sudo nvim /etc/systemd/network/20-ethernet.network

[Network]

Adress=212.122.4.7/24

Gateway=212.122.4.1

*logout*

sudo pacman -S hostapd

ip a

sudo nvim /etc/hostapd/hostapd.conf

*Cari: interface=wlan0*

*Cari: driver=nl80211*

*Cari: ssid=test*

*Cari: channel=7*

*Cari: auth_algs=1*

*Cari: wpa_passphrase=12346578*

*Cari: wpa_key_mgmt=WPA-PSK*

*Cari: wpa_pairwise=TKIP*

*Cari: rsn_pairwise*

sudo nvim /etc/systemd/network/02-wireless-ap.network

*ISI:*

[Match]

Name=wlan0

[Network]

Address=214.123.6.1/24

DHCPServer=yes

sudo nvim /etc/sysctl.d/99-custom.conf

*ISI*

net.ipv4.ip_forward=1

sudo sysctl --system

sudo systemctl enable --0 systemd-networkd

sudo systemctl enable --now hostapd

sudo firewall-cmd --new-zone=admin --permanent

sudo firewall-cmd --zone=admin --add-source=21.122.4.7 --permanent

sudo firewall-cmd --new-zone=operator --permanent

sudo firewall-cmd --new-zone=wifi --permanent

sudo firewall-cmd -new-zone=wifi -add-source=214.123.6.1/24

sudo firewall-cmd --zone=admin --add-service=ssh --permanent

sudo firewall-cmd --zone=admin --add-port={3306/tcp,6379/tcp,80/tcp,443/tcp} --permanent

sudo firewall-cmd --zone=operator --add-port={6379/tcp,80/tcp,443/tcp} --permanent

sudo firewall-cmd --zone=wifi --add-port={80/tcp,443/tcp} --permanent

sudo firewall-cmd --set default-zone=drop --permanent
