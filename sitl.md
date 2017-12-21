sitl

$http_proxy

- Installation Sudo
su - [root]
apt-get install sudo
adduser <user> sudo
logoff
logon
visudo
add Defaults env_keep += "ftp_proxy http_proxy https_proxy no_proxy"

- Virtualbox guest additions
http://dmesg.fr/categorie-logiciels/15-installer-virtualbox-guest-additions-debian
si erreur => /media/cdrom/VBoxLinuxAdditions.run (root)
Si debian failed to set up service vboxadd
=> http://pazpop.fr/installer-les-additions-invite-de-virtualbox-sur-debian-8/
apt-get update ; apt-get upgrade
apt-get install build-essential module-assistant
mount /media/cdrom
sh /media/cdrom/VBoxLinuxAdditions.run
Si erreur
apt-get install linux-headers-$(uname -r)
sh /media/cdrom/VBoxLinuxAdditions.run
sudo shutdown -r now



http://ardupilot.org/dev/docs/setting-up-sitl-on-linux.html

git config --global http.proxy http://fguillaume.ext:XXX@proxy-o:8089

# Ardupilot
git clone git://github.com/ArduPilot/ardupilot.git
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
git submodule update --init --recursive
if error, update urls in git config and relaunch command




# Deps
sudo apt-get install python-matplotlib python-serial python-wxgtk3.0 python-wxtools python-lxml
sudo apt-get install python-scipy python-opencv ccache gawk git python-pip python-pexpect
sudo pip install future pymavlink MAVProxy


export PATH=$PATH:$HOME/ardupilot/Tools/autotest
export PATH=/usr/lib/ccache:$PATH








sudo pip install zerorpc
sudo pip install dronekit




sim_veh


param load ../Tools/autotest/default_params/copter-ngs.parm











ARMING_CHECK    0
EK2_ENABLE      1
FRAME_TYPE      1
FRAME_CLASS     1
MAG_ENABLE      1
FS_THR_ENABLE   0
BATT_MONITOR    0
CH7_OPT         0
COMPASS_LEARN   0
FENCE_ENABLE    0
FRAME_CLASS     1
RC1_MAX         2000.000000
RC1_MIN         1000.000000
RC1_TRIM        1500.000000
RC2_MAX         2000.000000
RC2_MIN         1000.000000
RC2_TRIM        1500.000000
RC3_MAX         2000.000000
RC3_MIN         1000.000000
RC3_TRIM        1500.000000
RC4_MAX         2000.000000
RC4_MIN         1000.000000
RC4_TRIM        1500.000000
RC5_MAX         2000.000000
RC5_MIN         1000.000000
RC5_TRIM        1500.000000
RC6_MAX         2000.000000
RC6_MIN         1000.000000
RC6_TRIM        1500.000000
RC7_MAX         2000.000000
RC7_MIN         1000.000000
RC7_TRIM        1500.000000
RC8_MAX         2000.000000
RC8_MIN         1000.000000
RC8_TRIM        1500.000000
FLTMODE1        0
FLTMODE2        0
FLTMODE3        0
FLTMODE4        0
FLTMODE5        0
FLTMODE6        0
SUPER_SIMPLE    0
SIM_GPS_DELAY   0
SIM_ACC_RND     0
SIM_GYR_RND     0
SIM_WIND_SPD    0
SIM_WIND_TURB   0
SIM_BARO_RND    0
SIM_MAG_RND     0
SIM_GPS_GLITCH_X    0
SIM_GPS_GLITCH_Y    0
SIM_GPS_GLITCH_Z    0
# we need small INS_ACC offsets so INS is recognised as being calibrated
INS_ACCOFFS_X   0.001
INS_ACCOFFS_Y   0.001
INS_ACCOFFS_Z   0.001
INS_ACCSCAL_X   1.001
INS_ACCSCAL_Y   1.001
INS_ACCSCAL_Z   1.001
INS_ACC2OFFS_X   0.001
INS_ACC2OFFS_Y   0.001
INS_ACC2OFFS_Z   0.001
INS_ACC2SCAL_X   1.001
INS_ACC2SCAL_Y   1.001
INS_ACC2SCAL_Z   1.001
INS_ACC3OFFS_X   0.000
INS_ACC3OFFS_Y   0.000
INS_ACC3OFFS_Z   0.000
INS_ACC3SCAL_X   1.000
INS_ACC3SCAL_Y   1.000
INS_ACC3SCAL_Z   1.000
MOT_THST_EXPO 0.5
MOT_THST_HOVER  0.36
# flightmodes
# switch 1 Circle
# switch 2 LAND
# switch 3 RTL
# switch 4 Auto
# switch 5 Loiter
# switch 6 Stab
# STABILIZE     0 !
# ACRO                  1
# ALT_HOLD              2
# AUTO                  3 !
# GUIDED                4
# LOITER                5 !
# RTL                   6 !
# CIRCLE                7 !
# POSITION              8
# LAND                  9 !


