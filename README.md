# Qubes OS Setup

I'm using Qubes 4.1 with Debian 11 Template VMs.

This guide will help you to set up your development environment in Qubes OS.

To copy/move a file from a Qube to another, run

```bash
qvm-copy /path/of/file target-vm-name
qvm-move /path/of/file target-vm-name
```

## TemplateVM:

First, update your system and install some necessary packages:

```bash
sudo apt-get update
sudo apt-get upgrade

sudo apt-get install curl wget vim emacs qubes-snapd-helper fonts-noto-color-emoji
sudo apt-get install build-essential git cmake libboost-all-dev
sudo apt-get install python3 python3-pip default-jdk nodejs npm
```

Optionally, you can also install:

```bash
sudo apt-get pipx snapd libreoffice
```

If you want to use Github Desktop:

```bash
Run in an AppVM and move it to your TemplateVM: sudo wget https://github.com/shiftkey/desktop/releases/download/release-3.1.1-linux1/GitHubDesktop-linux-3.1.1-linux1.deb
sudo apt-get install gdebi-core
sudo gdebi GitHubDesktop-linux-3.1.1-linux1.deb
```

I recommend using [LibreWolf](https://librewolf.net/installation/debian/) as your default browser:

```bash
sudo apt update && sudo apt install -y wget gnupg lsb-release apt-transport-https ca-certificates
distro=$(if echo " una bookworm vanessa focal jammy bullseye vera uma " | grep -q " $(lsb_release -sc) "; then echo $(lsb_release -sc); else echo focal; fi)
Run in an AppVM and move it to your TemplateVM: wget -O- https://deb.librewolf.net/keyring.gpg | sudo gpg --dearmor -o /usr/share/keyrings/librewolf.gpg

sudo tee /etc/apt/sources.list.d/librewolf.sources << EOF > /dev/null
Types: deb
URIs: https://deb.librewolf.net
Suites: $distro
Components: main
Architectures: amd64
Signed-By: /usr/share/keyrings/librewolf.gpg
EOF

sudo apt update
sudo apt install librewolf -y
```

You can also use [Ungoogled Chromium](https://github.com/ungoogled-software/ungoogled-chromium-debian/blob/unportable/README.md#installing).


[Download binaries](https://ungoogled-software.github.io/ungoogled-chromium-binaries/releases/debian_unportable/amd64/100.0.4896.127-1) from an AppVM, move them to your TemplateVM and install them:

```bash
dpkg -i ungoogled-chromium_*.deb ungoogled-chromium-common_*.deb
```

To install extensions and themes on Ungoogled Chromium, go to `chrome://settings/` and drag and drop a .CRX theme/extension.
You can download Chrome extensions as .CRX using [CRX Extractor](http://crxextractor.com/).


## AppVM:

Now, inside your AppVM, install Node Version Manager (nvm), set the default version of Node.js, and install some global npm packages:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
source /home/user/.bashrc
nvm install node
nvm alias default node

sudo npm install -g yarn solc ganache-cli
```

Next, install [Foundry](https://getfoundry.sh/), a toolkit for Ethereum application development:

```bash
curl -L https://foundry.paradigm.xyz | bash
source /home/user/.bashrc
foundryup
```

### Install Solidity:

```bash
git clone https://github.com/ethereum/solidity.git
cd solidity
mkdir build
cd build
cmake ..
make
export PATH=$PATH:/home/user/solidity/build/solc
solc --version
```

### Python Setup:

It is recommended to use Python 3.9 (Comes by default with Debian 11).

Install `python3-venv` and create a virtual environment:

```bash
sudo apt-get install python3-venv
mkdir venvs
python3 -m venv venvs/brownie-env
source venvs/brownie-env/bin/activate
deactivate
```

Then, install some (un)necessary Python packages:

```bash
pip install eth-brownie web3 pytest requests termcolor
pip list
```

To uninstall all packages, you can use:

```bash
pip freeze | xargs pip uninstall -y
```

Finally, install some applications using Snap:

```bash
sudo snap install code --classic
sudo snap install gitkraken --classic
sudo snap install telegram-desktop
sudo snap install spotify
snap list
```

For other Snap applications, visit the [Snap Store](https://snapcraft.io/store).

If you want to use a VPN, follow the [Mullvad guide for Qubes 4](https://mullvad.net/en/help/qubes-os-4-and-mullvad-vpn/).

```
Happy coding!
```
