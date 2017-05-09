- Uncomment ll & la alias in .bashrc

- Proxy 
$http_proxy
App network

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
shutdown -r

- Minimize / maximize windows GNOME
sudo apt-get install gnome-tweak-tool
Tweak tool => windows enable maximize & minimize buttons

- Installation Sudo
su - [root]
apt-get install sudo
adduser <user> sudo
logoff
logon

- Installation tree
sudo apt-get install tree

- Installation postgres
sudo apt-get install postgresql postgresql-client
sudo apt-get install pgadmin3

  - Config pg_hba.conf
nano /etc/postgres/pg_hba.conf
local all postgres  peer
local all all       md5
/etc/init.d/postgres reload

  - Create DB user and DB
su -
su - postgres
createuser dbuser
createdb -O dbuser mypgdatabase

  - Connexion avec le compte postgres (chgt mdp compte dbuser)
su - postgres
psql
ALTER USER "dbuser" WITH PASSWORD 'dbuserPWD';


  - Connexion à la DB avec le compte user :
psql -d mypgdatabase -U mypguser



- Installation Sublime text
Download tar.bz2 file

as root :

mkdir /opt/sublime-text
cp bz2 file into the folder
tar -xvjf sublime_text_3_build_3126_x64.tar.bz2
ln -s /opt/sublime-text/sublime_text_3/sublime_text /usr/bin/sublime_text

- Install package control:
https://packagecontrol.io/docs/usage

=> CTRL + SHIFT + P, Install package
=> Install  
          Babel
          Oceanic next theme
          (Babel snippets)
          colorpicker
          DocBlockr
          emmet
          FileDiffs 
          "Git",
          gitgutter
          jsformat
      "Markdown Preview",
          "MarkdownEditing",
          Plain Task
          "SideBarEnhancements",
          SublimeLinter (sudo npm install -g eslint)
          sudo ln -s /opt/node/node-v6.9.1-linux-x64/lib/node_modules/jshint/bin/jshint /usr/local/bin/jshint
          SublimeLinter-eslint

          eslint airbnb


          

Configure BABEL :

for .js & .jsx files : view, syntax, pen all with current extension as..., Babel, JavaScript (Babel)
select color theme Babel/monokai

CONFIGURE COLORPICKER :
CTRL+SHIFT+C

Configure DocBlockR
TAB & SHIFT+TAB pour naviguer dans les comments

Configure emmet

Configure filediffs
Shortcut not working

Configure git
CTRL+SHIFT+P git

Configure gitgutter

Configure jsFormat 
CTRL+ALT+f

Configure eslint
airbnb guide
https://www.themarketingtechnologist.co/eslint-with-airbnb-javascript-style-guide-in-webstorm/
https://blog.javascripting.com/2015/09/07/fine-tuning-airbnbs-eslint-config/

(
  export PKG=eslint-config-airbnb;
  npm info "$PKG@latest" peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs npm install --save-dev "$PKG@latest"
)

npm install --save-dev babel-eslint
Add path to node in sublime-linter conf

eslint --init
AIRBNB JSON


https://blog.javascripting.com/2015/09/07/fine-tuning-airbnbs-eslint-config/


sublime conf :

## Settings - user

{
    "default_encoding": "UTF-8",
    "font_size": 10,
    "highlight_modified_tabs": true,
    "ignored_packages":
    [
        "Markdown Preview",
        "Vintage",
        "Markdown",
        "SideBarEnhancements",
        "Git",
        "Package Control"
    ],
    "tab_size": 2
}

## Keybindings - user
[
    { "keys": ["ctrl+œ"], "command": "show_panel", "args": {"panel": "console", "toggle": true} },
    {
        "keys": ["alt+shift+f1"],
        "command": "set_layout",
        "args":
        {
            "cols": [0.0, 1.0],
            "rows": [0.0, 1.0],
            "cells": [[0, 0, 1, 1]]
        }
    },
    {
        "keys": ["alt+shift+f2"],
        "command": "set_layout",
        "args":
        {
            "cols": [0.0, 0.5, 1.0],
            "rows": [0.0, 1.0],
            "cells": [[0, 0, 1, 1], [1, 0, 2, 1]]
        }
    },
    {
        "keys": ["alt+shift+f3"],
        "command": "set_layout",
        "args":
        {
            "cols": [0.0, 0.33, 0.66, 1.0],
            "rows": [0.0, 1.0],
            "cells": [[0, 0, 1, 1], [1, 0, 2, 1], [2, 0, 3, 1]]
        }
    },
    {
        "keys": ["alt+shift+f4"],
        "command": "set_layout",
        "args":
        {
            "cols": [0.0, 0.25, 0.5, 0.75, 1.0],
            "rows": [0.0, 1.0],
            "cells": [[0, 0, 1, 1], [1, 0, 2, 1], [2, 0, 3, 1], [3, 0, 4, 1]]
        }
    },
    {
        "keys": ["alt+shift+f8"],
        "command": "set_layout",
        "args":
        {
            "cols": [0.0, 1.0],
            "rows": [0.0, 0.5, 1.0],
            "cells": [[0, 0, 1, 1], [0, 1, 1, 2]]
        }
    },
    {
        "keys": ["alt+shift+f9"],
        "command": "set_layout",
        "args":
        {
            "cols": [0.0, 1.0],
            "rows": [0.0, 0.33, 0.66, 1.0],
            "cells": [[0, 0, 1, 1], [0, 1, 1, 2], [0, 2, 1, 3]]
        }
    },
    {
        "keys": ["alt+shift+f5"],
        "command": "set_layout",
        "args":
        {
            "cols": [0.0, 0.5, 1.0],
            "rows": [0.0, 0.5, 1.0],
            "cells":
            [
                [0, 0, 1, 1], [1, 0, 2, 1],
                [0, 1, 1, 2], [1, 1, 2, 2]
            ]
        }
    }
]


## markdown editing - GFM settings - user
{
    "draw_centered": false
}


## markdown editing - standard settings - user
{
    "draw_centered": false
}

## Plaintasks user settings
{
  "date_format": "(%Y-%m-%d %H:%M)"
}




- Setup JSHint Configuration on a Project by Project Basis



- Nodejs stack

Download tar.xz file
Copy & unzip in /opt/node
tar xvJf node-v6.9.1-linux-x64.tar.xz
ln -s /opt/node/node-v6.9.1-linux-x64/bin/node /usr/bin/node
ln -s /opt/node/node-v6.9.1-linux-x64/bin/npm /usr/bin/npm




---------------------------------------------
- Git install
sudo apt-get install
- Git config
git config --global user.name "Fred Guillaume"
git config --global user.email guillaume.frederic@gmail.com

git config --global core.editor sublime_text

- Hexo install
npm install -g hexo-cli
add link to /usr/local/bin for file <NODE_PATH>/lib/node_modules/hexo-cli/bin

- Hexo setup
hexo init <folder>
cd <folder>
npm install





- Node projet init
npm init -y
(
  export PKG=eslint-config-airbnb;
  npm info "$PKG@latest" peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs npm install --save-dev "$PKG@latest"
)
eslint --init
npm install --save-dev babel-eslint
npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react
npm install --save-dev mocha chai
npm install --save-dev jsdom
npm install --save immutable
npm install --save-dev chai-immutable
npm install --save-dev webpack webpack-dev-server
webpack.config.js

=> ./node_modules/webpack/bin/webpack.js
=> ./node_modules/webpack-dev-server/bin/webpack-dev-server.js (localhost:8080)

npm install --save react react-dom
npm install --save-dev react-hot-loader

npm install --save react-faux-dom
npm install --save d3

npm install --save react-addons-test-utils
npm install --save redux


-- CONFIG setup update

- Install yarn (use it instead of npm)
npm install -g yarn
sudo ln -s /opt/node/node-v6.9.1-linux-x64/lib/node_modules/yarn/bin/yarn /usr/local/bin/yarn





-- Creating custom neutrino preset
https://neutrino.js.org/presets/neutrino-lint-base/
https://davidwalsh.name/neutrino-linting


yarn add neutrino-lint-base

