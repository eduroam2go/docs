# Building UETN eduroam2go image using imagebuilder - GL.iNet GL-MT1300 #

I'm doing all of this on a desktop running Ubuntu 22.04 as a non-root user.

Reference: https://openwrt.org/docs/guide-user/additional-software/imagebuilder

The instructions say to install the following packages for Debian/Ubuntu:

```
sudo apt install build-essential libncurses-dev libncursesw-dev \
zlib1g-dev gawk git gettext libssl-dev xsltproc rsync wget unzip python3
```

But `libncursesw-dev` isn't a valid package. I installed `libncursesw5-dev` instead.

```
cd ~

wget https://archive.openwrt.org/releases/22.03.5/targets/ramips/mt7621/openwrt-imagebuilder-22.03.5-ramips-mt7621.Linux-x86_64.tar.xz

tar -J -x -f openwrt-imagebuilder-22.03.5-ramips-mt7621.Linux-x86_64.tar.xz 

cd openwrt-imagebuilder-22.03.5-ramips-mt7621.Linux-x86_64/

#Link to existing files folder in my home directory
ln -s ~/files files

make image \
PROFILE="glinet_gl-mt1300" \
PACKAGES="nano -wpad-basic-wolfssl -iw iw-full hostapd-common wpad-wolfssl luci-proto-wireguard luci-app-wireguard openwisp-config openwisp-monitoring luci-app-openwisp luci luci-app-firewall luci-app-opkg luci-base luci-lib-base luci-lib-ip luci-lib-jsonc luci-lib-nixio luci-mod-admin-full luci-mod-network luci-mod-status luci-mod-system luci-proto-ipv6 luci-proto-ppp luci-ssl luci-theme-bootstrap" \
FILES="files"

#Copy the completed image to my workstation
scp bin/targets/ramips/mt7621/openwrt-22.03.5-ramips-mt7621-glinet_gl-mt1300-squashfs-sysupgrade.bin username@(host):~

#now flash openwrt-22.03.5-ramips-mt7621-glinet_gl-mt1300-squashfs-sysupgrade.bin to the GL.iNet GL-MT1300
# when the flashing uncheck the "save config" option
# reboot

```



== Extra Notes ==

=== Info about "Stock" OpenWRT Image ===
Image - https://archive.openwrt.org/releases/22.03.5/targets/ramips/mt7621/openwrt-22.03.5-ramips-mt7621-glinet_gl-mt1300-squashfs-sysupgrade.bin

==== Stock Packages ====
```
root@OpenWrt:/# echo -e "$(opkg list-installed | sed -e "s/\s.*$//")\n"
base-files
busybox
ca-bundle
cgi-io
dnsmasq
dropbear
firewall4
fstools
fwtool
getrandom
hostapd-common
iw
iwinfo
jansson4
jshn
jsonfilter
kernel
kmod-cfg80211
kmod-crypto-aead
kmod-crypto-ccm
kmod-crypto-cmac
kmod-crypto-crc32c
kmod-crypto-ctr
kmod-crypto-gcm
kmod-crypto-gf128
kmod-crypto-ghash
kmod-crypto-hash
kmod-crypto-hmac
kmod-crypto-manager
kmod-crypto-null
kmod-crypto-rng
kmod-crypto-seqiv
kmod-crypto-sha256
kmod-gpio-button-hotplug
kmod-hwmon-core
kmod-leds-gpio
kmod-lib-crc-ccitt
kmod-lib-crc32c
kmod-mac80211
kmod-mt76-connac
kmod-mt76-core
kmod-mt7615-common
kmod-mt7615-firmware
kmod-mt7615e
kmod-nf-conntrack
kmod-nf-conntrack6
kmod-nf-flow
kmod-nf-log
kmod-nf-log6
kmod-nf-nat
kmod-nf-reject
kmod-nf-reject6
kmod-nfnetlink
kmod-nft-core
kmod-nft-fib
kmod-nft-nat
kmod-nft-offload
kmod-nls-base
kmod-ppp
kmod-pppoe
kmod-pppox
kmod-slhc
kmod-usb-core
kmod-usb-xhci-hcd
kmod-usb-xhci-mtk
kmod-usb3
libblobmsg-json20220515
libc
libgcc1
libiwinfo-data
libiwinfo-lua
libiwinfo20210430
libjson-c5
libjson-script20220515
liblua5.1.5
liblucihttp-lua
liblucihttp0
libmnl0
libnftnl11
libnl-tiny1
libpthread
libubox20220515
libubus-lua
libubus20220601
libuci20130104
libuclient20201210
libucode20220812
libustream-wolfssl20201210
libwolfssl5.5.4.ee39414e
logd
lua
luci
luci-app-firewall
luci-app-opkg
luci-base
luci-lib-base
luci-lib-ip
luci-lib-jsonc
luci-lib-nixio
luci-mod-admin-full
luci-mod-network
luci-mod-status
luci-mod-system
luci-proto-ipv6
luci-proto-ppp
luci-ssl
luci-theme-bootstrap
mtd
netifd
nftables-json
odhcp6c
odhcpd-ipv6only
openwrt-keyring
opkg
ppp
ppp-mod-pppoe
procd
procd-seccomp
procd-ujail
px5g-wolfssl
rpcd
rpcd-mod-file
rpcd-mod-iwinfo
rpcd-mod-luci
rpcd-mod-rrdns
ubi-utils
ubox
ubus
ubusd
uci
uclient-fetch
ucode
ucode-mod-fs
ucode-mod-ubus
ucode-mod-uci
uhttpd
uhttpd-mod-ubus
urandom-seed
urngd
usign
wireless-regdb
wpad-basic-wolfssl
```

==== Stock uci Config ====
```
root@OpenWrt:/# uci show
dhcp.@dnsmasq[0]=dnsmasq
dhcp.@dnsmasq[0].domainneeded='1'
dhcp.@dnsmasq[0].boguspriv='1'
dhcp.@dnsmasq[0].filterwin2k='0'
dhcp.@dnsmasq[0].localise_queries='1'
dhcp.@dnsmasq[0].rebind_protection='1'
dhcp.@dnsmasq[0].rebind_localhost='1'
dhcp.@dnsmasq[0].local='/lan/'
dhcp.@dnsmasq[0].domain='lan'
dhcp.@dnsmasq[0].expandhosts='1'
dhcp.@dnsmasq[0].nonegcache='0'
dhcp.@dnsmasq[0].authoritative='1'
dhcp.@dnsmasq[0].readethers='1'
dhcp.@dnsmasq[0].leasefile='/tmp/dhcp.leases'
dhcp.@dnsmasq[0].resolvfile='/tmp/resolv.conf.d/resolv.conf.auto'
dhcp.@dnsmasq[0].nonwildcard='1'
dhcp.@dnsmasq[0].localservice='1'
dhcp.@dnsmasq[0].ednspacket_max='1232'
dhcp.lan=dhcp
dhcp.lan.interface='lan'
dhcp.lan.start='100'
dhcp.lan.limit='150'
dhcp.lan.leasetime='12h'
dhcp.lan.dhcpv4='server'
dhcp.lan.dhcpv6='server'
dhcp.lan.ra='server'
dhcp.lan.ra_slaac='1'
dhcp.lan.ra_flags='managed-config' 'other-config'
dhcp.wan=dhcp
dhcp.wan.interface='wan'
dhcp.wan.ignore='1'
dhcp.odhcpd=odhcpd
dhcp.odhcpd.maindhcp='0'
dhcp.odhcpd.leasefile='/tmp/hosts/odhcpd'
dhcp.odhcpd.leasetrigger='/usr/sbin/odhcpd-update'
dhcp.odhcpd.loglevel='4'
dropbear.@dropbear[0]=dropbear
dropbear.@dropbear[0].PasswordAuth='on'
dropbear.@dropbear[0].RootPasswordAuth='on'
dropbear.@dropbear[0].Port='22'
firewall.@defaults[0]=defaults
firewall.@defaults[0].syn_flood='1'
firewall.@defaults[0].input='ACCEPT'
firewall.@defaults[0].output='ACCEPT'
firewall.@defaults[0].forward='REJECT'
firewall.@zone[0]=zone
firewall.@zone[0].name='lan'
firewall.@zone[0].network='lan'
firewall.@zone[0].input='ACCEPT'
firewall.@zone[0].output='ACCEPT'
firewall.@zone[0].forward='ACCEPT'
firewall.@zone[1]=zone
firewall.@zone[1].name='wan'
firewall.@zone[1].network='wan' 'wan6'
firewall.@zone[1].input='REJECT'
firewall.@zone[1].output='ACCEPT'
firewall.@zone[1].forward='REJECT'
firewall.@zone[1].masq='1'
firewall.@zone[1].mtu_fix='1'
firewall.@forwarding[0]=forwarding
firewall.@forwarding[0].src='lan'
firewall.@forwarding[0].dest='wan'
firewall.@rule[0]=rule
firewall.@rule[0].name='Allow-DHCP-Renew'
firewall.@rule[0].src='wan'
firewall.@rule[0].proto='udp'
firewall.@rule[0].dest_port='68'
firewall.@rule[0].target='ACCEPT'
firewall.@rule[0].family='ipv4'
firewall.@rule[1]=rule
firewall.@rule[1].name='Allow-Ping'
firewall.@rule[1].src='wan'
firewall.@rule[1].proto='icmp'
firewall.@rule[1].icmp_type='echo-request'
firewall.@rule[1].family='ipv4'
firewall.@rule[1].target='ACCEPT'
firewall.@rule[2]=rule
firewall.@rule[2].name='Allow-IGMP'
firewall.@rule[2].src='wan'
firewall.@rule[2].proto='igmp'
firewall.@rule[2].family='ipv4'
firewall.@rule[2].target='ACCEPT'
firewall.@rule[3]=rule
firewall.@rule[3].name='Allow-DHCPv6'
firewall.@rule[3].src='wan'
firewall.@rule[3].proto='udp'
firewall.@rule[3].dest_port='546'
firewall.@rule[3].family='ipv6'
firewall.@rule[3].target='ACCEPT'
firewall.@rule[4]=rule
firewall.@rule[4].name='Allow-MLD'
firewall.@rule[4].src='wan'
firewall.@rule[4].proto='icmp'
firewall.@rule[4].src_ip='fe80::/10'
firewall.@rule[4].icmp_type='130/0' '131/0' '132/0' '143/0'
firewall.@rule[4].family='ipv6'
firewall.@rule[4].target='ACCEPT'
firewall.@rule[5]=rule
firewall.@rule[5].name='Allow-ICMPv6-Input'
firewall.@rule[5].src='wan'
firewall.@rule[5].proto='icmp'
firewall.@rule[5].icmp_type='echo-request' 'echo-reply' 'destination-unreachable' 'packet-too-big' 'time-exceeded' 'bad-header' 'unknown-header-type' 'router-solicitation' 'neighbour-solicitation' 'router-advertisement' 'neighbour-advertisement'
firewall.@rule[5].limit='1000/sec'
firewall.@rule[5].family='ipv6'
firewall.@rule[5].target='ACCEPT'
firewall.@rule[6]=rule
firewall.@rule[6].name='Allow-ICMPv6-Forward'
firewall.@rule[6].src='wan'
firewall.@rule[6].dest='*'
firewall.@rule[6].proto='icmp'
firewall.@rule[6].icmp_type='echo-request' 'echo-reply' 'destination-unreachable' 'packet-too-big' 'time-exceeded' 'bad-header' 'unknown-header-type'
firewall.@rule[6].limit='1000/sec'
firewall.@rule[6].family='ipv6'
firewall.@rule[6].target='ACCEPT'
firewall.@rule[7]=rule
firewall.@rule[7].name='Allow-IPSec-ESP'
firewall.@rule[7].src='wan'
firewall.@rule[7].dest='lan'
firewall.@rule[7].proto='esp'
firewall.@rule[7].target='ACCEPT'
firewall.@rule[8]=rule
firewall.@rule[8].name='Allow-ISAKMP'
firewall.@rule[8].src='wan'
firewall.@rule[8].dest='lan'
firewall.@rule[8].dest_port='500'
firewall.@rule[8].proto='udp'
firewall.@rule[8].target='ACCEPT'
luci.main=core
luci.main.lang='auto'
luci.main.mediaurlbase='/luci-static/bootstrap'
luci.main.resourcebase='/luci-static/resources'
luci.main.ubuspath='/ubus/'
luci.flash_keep=extern
luci.flash_keep.uci='/etc/config/'
luci.flash_keep.dropbear='/etc/dropbear/'
luci.flash_keep.openvpn='/etc/openvpn/'
luci.flash_keep.passwd='/etc/passwd'
luci.flash_keep.opkg='/etc/opkg.conf'
luci.flash_keep.firewall='/etc/firewall.user'
luci.flash_keep.uploads='/lib/uci/upload/'
luci.languages=internal
luci.sauth=internal
luci.sauth.sessionpath='/tmp/luci-sessions'
luci.sauth.sessiontime='3600'
luci.ccache=internal
luci.ccache.enable='1'
luci.themes=internal
luci.themes.Bootstrap='/luci-static/bootstrap'
luci.themes.BootstrapDark='/luci-static/bootstrap-dark'
luci.themes.BootstrapLight='/luci-static/bootstrap-light'
luci.apply=internal
luci.apply.rollback='90'
luci.apply.holdoff='4'
luci.apply.timeout='5'
luci.apply.display='1.5'
luci.diag=internal
luci.diag.dns='openwrt.org'
luci.diag.ping='openwrt.org'
luci.diag.route='openwrt.org'
network.loopback=interface
network.loopback.device='lo'
network.loopback.proto='static'
network.loopback.ipaddr='127.0.0.1'
network.loopback.netmask='255.0.0.0'
network.globals=globals
network.globals.packet_steering='1'
network.globals.ula_prefix='fd20:d26b:4a99::/48'
network.@device[0]=device
network.@device[0].name='br-lan'
network.@device[0].type='bridge'
network.@device[0].ports='lan1' 'lan2'
network.lan=interface
network.lan.device='br-lan'
network.lan.proto='static'
network.lan.ipaddr='192.168.1.1'
network.lan.netmask='255.255.255.0'
network.lan.ip6assign='60'
network.wan=interface
network.wan.device='wan'
network.wan.proto='dhcp'
network.wan6=interface
network.wan6.device='wan'
network.wan6.proto='dhcpv6'
rpcd.@rpcd[0]=rpcd
rpcd.@rpcd[0].socket='/var/run/ubus/ubus.sock'
rpcd.@rpcd[0].timeout='30'
rpcd.@login[0]=login
rpcd.@login[0].username='root'
rpcd.@login[0].password='$p$root'
rpcd.@login[0].read='*'
rpcd.@login[0].write='*'
system.@system[0]=system
system.@system[0].hostname='OpenWrt'
system.@system[0].timezone='UTC'
system.@system[0].ttylogin='0'
system.@system[0].log_size='64'
system.@system[0].urandom_seed='0'
system.@system[0].compat_version='1.1'
system.ntp=timeserver
system.ntp.enabled='1'
system.ntp.enable_server='0'
system.ntp.server='0.openwrt.pool.ntp.org' '1.openwrt.pool.ntp.org' '2.openwrt.pool.ntp.org' '3.openwrt.pool.ntp.org'
ucitrack.@network[0]=network
ucitrack.@network[0].init='network'
ucitrack.@network[0].affects='dhcp'
ucitrack.@wireless[0]=wireless
ucitrack.@wireless[0].affects='network'
ucitrack.@firewall[0]=firewall
ucitrack.@firewall[0].init='firewall'
ucitrack.@firewall[0].affects='luci-splash' 'qos' 'miniupnpd'
ucitrack.@olsr[0]=olsr
ucitrack.@olsr[0].init='olsrd'
ucitrack.@dhcp[0]=dhcp
ucitrack.@dhcp[0].init='dnsmasq'
ucitrack.@dhcp[0].affects='odhcpd'
ucitrack.@odhcpd[0]=odhcpd
ucitrack.@odhcpd[0].init='odhcpd'
ucitrack.@dropbear[0]=dropbear
ucitrack.@dropbear[0].init='dropbear'
ucitrack.@httpd[0]=httpd
ucitrack.@httpd[0].init='httpd'
ucitrack.@fstab[0]=fstab
ucitrack.@fstab[0].exec='/sbin/block mount'
ucitrack.@qos[0]=qos
ucitrack.@qos[0].init='qos'
ucitrack.@system[0]=system
ucitrack.@system[0].init='led'
ucitrack.@system[0].exec='/etc/init.d/log reload'
ucitrack.@system[0].affects='luci_statistics' 'dhcp'
ucitrack.@luci_splash[0]=luci_splash
ucitrack.@luci_splash[0].init='luci_splash'
ucitrack.@upnpd[0]=upnpd
ucitrack.@upnpd[0].init='miniupnpd'
ucitrack.@ntpclient[0]=ntpclient
ucitrack.@ntpclient[0].init='ntpclient'
ucitrack.@samba[0]=samba
ucitrack.@samba[0].init='samba'
ucitrack.@tinyproxy[0]=tinyproxy
ucitrack.@tinyproxy[0].init='tinyproxy'
uhttpd.main=uhttpd
uhttpd.main.listen_http='0.0.0.0:80' '[::]:80'
uhttpd.main.listen_https='0.0.0.0:443' '[::]:443'
uhttpd.main.redirect_https='0'
uhttpd.main.home='/www'
uhttpd.main.rfc1918_filter='1'
uhttpd.main.max_requests='3'
uhttpd.main.max_connections='100'
uhttpd.main.cert='/etc/uhttpd.crt'
uhttpd.main.key='/etc/uhttpd.key'
uhttpd.main.cgi_prefix='/cgi-bin'
uhttpd.main.lua_prefix='/cgi-bin/luci=/usr/lib/lua/luci/sgi/uhttpd.lua'
uhttpd.main.script_timeout='60'
uhttpd.main.network_timeout='30'
uhttpd.main.http_keepalive='20'
uhttpd.main.tcp_keepalive='1'
uhttpd.main.ubus_prefix='/ubus'
uhttpd.defaults=cert
uhttpd.defaults.days='730'
uhttpd.defaults.key_type='ec'
uhttpd.defaults.bits='2048'
uhttpd.defaults.ec_curve='P-256'
uhttpd.defaults.country='ZZ'
uhttpd.defaults.state='Somewhere'
uhttpd.defaults.location='Unknown'
uhttpd.defaults.commonname='OpenWrt'
wireless.radio0=wifi-device
wireless.radio0.type='mac80211'
wireless.radio0.path='1e140000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0'
wireless.radio0.channel='36'
wireless.radio0.band='5g'
wireless.radio0.htmode='VHT80'
wireless.radio0.disabled='1'
wireless.default_radio0=wifi-iface
wireless.default_radio0.device='radio0'
wireless.default_radio0.network='lan'
wireless.default_radio0.mode='ap'
wireless.default_radio0.ssid='OpenWrt'
wireless.default_radio0.encryption='none'
wireless.radio1=wifi-device
wireless.radio1.type='mac80211'
wireless.radio1.path='1e140000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0+1'
wireless.radio1.channel='36'
wireless.radio1.band='5g'
wireless.radio1.htmode='VHT80'
wireless.radio1.disabled='1'
wireless.default_radio1=wifi-iface
wireless.default_radio1.device='radio1'
wireless.default_radio1.network='lan'
wireless.default_radio1.mode='ap'
wireless.default_radio1.ssid='OpenWrt'
wireless.default_radio1.encryption='none'
root@OpenWrt:/# 
```
