# dotfiles

Scripts and configurations for setting up my Linux environment.

## Operating system installation

Download Ubuntu Desktop from <https://ubuntu.com/download/desktop>.
Use <https://www.balena.io/etcher/> to make bootable USB drive.

It is recommended to install latest LTS version. At the moment of writing LTS version is `18.04.3`.

During installation enable disk encryption.

### Various software packages

```bash
sudo apt update
sudo apt upgrade

sudo apt install \
 build-essential \
 curl \
 wget \
 httpie \
 file \
 git \
 p7zip \
 htop \
 synaptic \
 tilix \
 vim \
 firefox \
 -y
 ```

### Video drivers

#### NVIDIA

In "Software & Updates" settings / "Additional Drivers" select latest NVIDIA
driver package; apply; reboot.

This will install both graphics and OpenCL drivers.

At the moment of writing NVIDIA driver package is `nvidia-driver-435`.

**NOTE:** When using AMD eGPU to drive external monitor, I found that it is best to use integrated Intel card and not the NVIDIA card.
In "NVIDIA X Server" application, in "PRIME Profiles" select "Intel (Power Saving Mode)" and re-login.

#### Intel OpenCL

Install OpenCL drivers for integrated Intel video card.

<https://github.com/intel/compute-runtime>

```bash
mkdir neo

cd neo
wget https://github.com/intel/compute-runtime/releases/download/19.46.14807/intel-gmmlib_19.3.4_amd64.deb
wget https://github.com/intel/compute-runtime/releases/download/19.46.14807/intel-igc-core_1.0.2878_amd64.deb
wget https://github.com/intel/compute-runtime/releases/download/19.46.14807/intel-igc-opencl_1.0.2878_amd64.deb
wget https://github.com/intel/compute-runtime/releases/download/19.46.14807/intel-opencl_19.46.14807_amd64.deb
wget https://github.com/intel/compute-runtime/releases/download/19.46.14807/intel-ocloc_19.46.14807_amd64.deb

wget https://github.com/intel/compute-runtime/releases/download/19.46.14807/ww46.sum
sha256sum -c ww46.sum

sudo dpkg -i *.deb
```

#### AMD OpenCL

Install OpenCL drivers for external AMD video card.

Download drivers package from <https://www.amd.com/en/support/>.

Download link for AMD RX 580: <https://www.amd.com/en/support/graphics/radeon-500-series/radeon-rx-500-series/radeon-rx-580>

Uncompress the package.

From the drivers directory, run

```bash
./amdgpu-install -y --opencl=pal,legacy --headless
```

### SSH Key

Generate SSH key:

```bash
ssh-keygen -f ~/.ssh/some_name -t rsa -b 4096 -C "some_email"
echo "Public key:"
cat ~/.ssh/some_name.pub
```

### Oh My ZSH

<https://ohmyz.sh/>

```bash
sudo apt install -y zsh git
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### Papirus Icons

<https://github.com/PapirusDevelopmentTeam/papirus-icon-theme>

```bash
sudo add-apt-repository ppa:papirus/papirus
sudo apt update
sudo apt install -y papirus-icon-theme gnome-tweaks
```

### Mount Windows Shares

Example of using a Windows file share:

```bash
sudo apt install -y cifs-utils
sudo mount -t cifs //192.168.1.172/data /mnt/win-data -o user=testuser
```

### UI Tweaks


#### Pop GTK theme

<https://github.com/pop-os/gtk-theme>

```bash
sudo apt install sassc meson libglib2.0-dev

# Optional:
sudo apt install inkscape optipng

# Optional:
sudo apt remove pop-gtk-theme
sudo rm -rf /usr/share/themes/Pop*
rm -rf ~/.local/share/themes/Pop*
rm -rf ~/.themes/Pop*

git clone https://github.com/pop-os/gtk-theme
cd gtk-theme

meson build && cd build
ninja
sudo ninja install
```

#### GNOME Extensions

<https://extensions.gnome.org>

- [Caffeine](https://extensions.gnome.org/extension/517/caffeine/)
- [Clipboard Indicator](https://extensions.gnome.org/extension/779/clipboard-indicator/)
- [cpufreq](https://extensions.gnome.org/extension/1082/cpufreq/)
- [Dash to Panel](https://extensions.gnome.org/extension/1160/dash-to-panel/)
- [Dynamic Panel Transparency](https://extensions.gnome.org/extension/1011/dynamic-panel-transparency/)
- [Topicons Plus](https://extensions.gnome.org/extension/1031/topicons/)
- [Vitals](https://extensions.gnome.org/extension/1460/vitals/)

#### UI settings

GNOME Tweaks:

- Set UI theme to `Pop`
- Set icons theme to `Papirus`

Increase cursor size:

```bash
gsettings set org.gnome.desktop.interface cursor-size 32
```


### Cleanup

Remove unused fonts (`synaptic`'s "Fonts" section) - Ubuntu installs lots of them.

Remove Snap:

```bash
sudo apt remove snapd
```


## Software

### Bitwarden

<https://bitwarden.com/>

```bash
cd ~/Downloads
wget https://vault.bitwarden.com/download/?app=desktop&platform=linux&variant=deb
mv 'index.html?app=desktop&platform=linux&variant=deb' bitwarden.deb
sudo apt install ./bitwarden.deb
rm bitwarden.deb
```

### Firefox

- enable FireFox sync
- set DuckDuckGo as default search engine
- extensions (should already be enabled via FireFox sync):
  - uBlock Origin
  - Bitwarden
  - HTTPS Everywhere
  - Fixed Zoom
  - Enchancer for YouTube
  - h264ify

### GitHub

- Sign in to <https://github.com>
- At <https://github.com/settings/keys>, upload the SSH key created above

### VisualStudio Code

- <https://code.visualstudio.com/>
- Download `.deb`, install

```bash
mkdir -p ~/.config/Code/User
cp .config/Code/User/settings.json ~/.config/Code/User/
```

#### PlantUML Prerequisites

```bash
sudo apt install graphviz openjdk-11-jre-headless
```

### Sublime Text, Sublime Merge

- <https://www.sublimetext.com>
- <https://www.sublimemerge.com>

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime.list
sudo apt update
sudo apt install -y sublime-text sublime-merge
```

```bash
mkdir -p ~/.config/sublime-text-3/Packages/User
cp .config/sublime-text-3/Packages/User/Preferences.sublime-settings ~/.config/sublime-text-3/Packages/User
```

### BeyondCompare

- <https://scootersoftware.com>
- Download `.deb`, install

### Docker

- <https://docs.docker.com/install/linux/docker-ce/ubuntu>
- <https://docs.docker.com/install/linux/linux-postinstall>

```bash
sudo apt remove -y docker docker-engine docker-compose docker.io containerd runc

sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update

sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose

sudo systemctl enable docker
sudo usermod -aG docker $USER
```

Re-login, then verify the installation:

```bash
docker run --rm hello-world
```

### .NET Core SDK

<https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-install-script>

```bash
sudo apt install -y apt-transport-https

wget https://dot.net/v1/dotnet-install.sh
chmod +x ./dotnet-install.sh

sudo ./dotnet-install.sh --version 2.2.402 --install-dir /opt/dotnet
sudo ./dotnet-install.sh --version 3.0.100 --install-dir /opt/dotnet

echo 'export PATH="$PATH:/opt/dotnet/:$HOME/.dotnet/tools"\nexport DOTNET_ROOT=/opt/dotnet' | sudo tee /etc/profile.d/dotnet-path.sh
source /etc/profile.d/dotnet-path.sh
```

Verify the installation:

```bash
dotnet --info
```

### PowerShell

<https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-6>

```bash
wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo apt install ./packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

sudo apt update
sudo apt install -y powershell
```

### Rust

- <https://www.rust-lang.org>
- <https://www.rust-lang.org/tools/install>

```bash
curl https://sh.rustup.rs -sSf | sh
```

### Haskell

<https://docs.haskellstack.org>

```bash
curl -sSL https://get.haskellstack.org/ | sh
```

### Scala/SBT

- <https://www.scala-lang.org/>
- <https://www.scala-sbt.org/download.html>

```bash
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | sudo apt-key add

sudo apt update
sudo apt install sbt
```

### Erlang+Elixir

<https://elixir-lang.org/install.html>

```bash
wget https://packages.erlang-solutions.com/erlang-solutions_2.0_all.deb
sudo dpkg -i erlang-solutions_2.0_all.deb
rm erlang-solutions_2.0_all.deb

sudo apt update
sudo apt install esl-erlang elixir
```

### NodeJS+NPM+Yarn

```bash
wget https://nodejs.org/dist/v12.13.0/node-v12.13.0-linux-x64.tar.xz
tar xvf node-v12.13.0-linux-x64.tar.xz

sudo mv node-v12.13.0-linux-x64 /opt/nodejs
echo 'export PATH="$PATH:/opt/nodejs/bin"' | sudo tee /etc/profile.d/nodejs-path.sh
```

### [?] R

- <https://www.r-project.org/>
- <https://linuxize.com/post/how-to-install-r-on-ubuntu-18-04/>
- <https://www.digitalocean.com/community/tutorials/how-to-install-r-on-ubuntu-18-04>

```bash
sudo apt install apt-transport-https software-properties-common

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/'

sudo apt install r-base
```

### Postman+Newman

<https://www.getpostman.com/>

Postman:

```bash
wget https://dl.pstmn.io/download/latest/linux64 -O postman.tgz
tar xvfz postman.tgz
sudo mv Postman /opt/postman

cp .local/share/applications/postman.desktop ~/.local/share/applications/
```

Newman:

```bash
sudo -i
source /etc/profile.d/nodejs-path.sh
npm install -g newman
```

### Azure CLI

<https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-apt?view=azure-cli-latest>

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

Initialize:

```bash
az login
```

### Google Cloud Platform SDK

<https://cloud.google.com/sdk/>
<https://cloud.google.com/sdk/docs/quickstart-debian-ubuntu>

```bash
# Add the Cloud SDK distribution URI as a package source
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

# Import the Google Cloud Platform public key
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -

# Update the package list and install the Cloud SDK
sudo apt update && sudo apt install google-cloud-sdk kubectl
```

Initialize:

```bash
gcloud init
```

After logged in, navigate to <https://console.cloud.google.com/kubernetes/list>, select cluster and view command line to connect:

```bash
gcloud container clusters get-credentials dev-cluster --zone <your-zone> --project <your-project>
```

### [?] Pulumi

<https://www.pulumi.com/docs/get-started/install/>

```bash
curl -fsSL https://get.pulumi.com | sh
```

### JetBrains Rider

- download from <https://www.jetbrains.com/rider/download>
- unpack and move to `/opt/jetbrains_rider`

### JetBrains PyCharm

- download from <https://www.jetbrains.com/pycharm/download/>
- unpack and move to `/opt/jetbrains_pycharm`

### JetBrains IDEA

- download from <https://www.jetbrains.com/idea/download>
- unpack and move to `/opt/jetbrains_idea`

### JetBrains DataGrip

- download from <https://www.jetbrains.com/datagrip/download>
- unpack and move to `/opt/jetbrains_datagrip`

### Fonts

Download Input fonts: <https://input.fontbureau.com/download/>

Download IBM Plex: <https://www.ibm.com/plex/>

```bash
wget https://github.com/IBM/plex/releases/download/v3.0.0/OpenType.zip
```

Download Fira Code: <https://github.com/tonsky/FiraCode>

Download Adobe Source fonts:

```bash
wget https://github.com/adobe-fonts/source-code-pro/releases/download/2.030R-ro%2F1.050R-it/source-code-pro-2.030R-ro-1.050R-it.zip
wget https://github.com/adobe-fonts/source-serif-pro/releases/download/3.000R/source-serif-pro-3.000R.zip
wget https://github.com/adobe-fonts/source-sans-pro/releases/download/3.006R/source-sans-pro-3.006R.zip
```

... extract all of the above

```bash
mkdir ~/.local/share/fonts

mv OpenType ~/.local/share/fonts/IBM_Plex
mv Input-Font/Input_Fonts ~/.local/share/fonts/
mv FiraCode_2/otf ~/.local/share/fonts/FiraCode
mv source-code-pro-2.030R-ro-1.050R-it/OTF ~/.local/share/fonts/SourceCode
mv source-sans-pro-3.006R/OTF ~/.local/share/fonts/SourceSans
mv source-serif-pro-3.000R/OTF ~/.local/share/fonts/SourceSerif

fc-cache -f -r -v
```

### Remmina

<https://remmina.org>

```bash
sudo apt-add-repository ppa:remmina-ppa-team/remmina-next
sudo apt update
sudo apt install -y remmina remmina-plugin-rdp remmina-plugin-secret remmina-plugin-spice
```

### Slack

Download: <https://slack.com/intl/en-gb/downloads/linux>

### [?] MailSpring

Download: <https://updates.getmailspring.com/download?platform=linuxDeb>

### [?] MineTime

MineTime calendar: <https://minetime.ai/>

```bash
wget https://minetime-deploy.herokuapp.com/download/linux_deb_64 -O minetime.deb
sudo dpkg -i minetime.deb
rm minetime.deb
```

**NOTE**: For FastMail CalDAV use server URL
`https://caldav.fastmail.com/dav/calendars/user/<username@fastmail.in>/`.

### Electron Apps

#### Prerequisite: `nativefier`

```bash
sudo -i
source /etc/profile.d/nodejs-path.sh
npm install -g nativefier
```

#### Notion

```bash
nativefier --name Notion --zoom 1.25 https://www.notion.so
sudo mv notion-linux-x64 /opt/notion
```

```bash
sudo cat .local/share/applications/icons/notion.png > /opt/notion/resources/app/icon.png
cp .local/share/applications/notion.desktop ~/.local/share/applications/
```

#### Remember the Milk

```bash
nativefier --name remember_the_milk --zoom 1.25 https://www.rememberthemilk.com/app
sudo mv remember-the-milk-linux-x64 /opt/remember_the_milk
```

```bash
sudo cat .local/share/applications/icons/remember_the_milk.png > /opt/remember_the_milk/resources/app/icon.png
cp .local/share/applications/remember_the_milk.desktop ~/.local/share/applications/
```

#### FastMail

```bash
nativefier --name fastmail --zoom 1.25 https://www.fastmail.com
sudo mv fastmail-linux-x64 /opt/fastmail
```

```bash
cp .local/share/applications/fastmail.desktop ~/.local/share/applications/
```

### Graphics and Photography

#### DisplayCal

Ubuntu has `dispcalgui` package built-in, but it is an older version.

Download latest from <https://displaycal.net/>, install.

#### Darktable

Ubuntu has `darktable` package built-in, but it is an older version.

<https://software.opensuse.org/download.html?project=graphics:darktable&package=darktable>

```bash
sudo sh -c "echo 'deb http://download.opensuse.org/repositories/graphics:/darktable/xUbuntu_18.04/ /' > /etc/apt/sources.list.d/graphics:darktable.list"
wget -nv https://download.opensuse.org/repositories/graphics:darktable/xUbuntu_18.04/Release.key -O Release.key
sudo apt-key add - < Release.key
sudo apt-get update
sudo apt-get install darktable
```

To verify that OpenCL is supported:

```bash
sudo apt install -y clinfo
clinfo
```

In Darktable preferences OpenCL support should be enabled.

#### Rapid Photo Downloader, GIMP, Hugin

```bash
sudo apt install -y rapid-photo-downloader libmediainfo0v5 gimp hugin
```

### Media

#### Audio editing

```bash
sudo apt install -y audacity
```

#### Video player, codecs

```bash
sudo apt install -y ffmpeg
sudo apt install -y mpv
```

#### Open Broadcaster Software

<https://obsproject.com/>

```bash
sudo apt install -y ffmpeg

sudo add-apt-repository ppa:obsproject/obs-studio
sudo apt install -y obs-studio
```

**Note:** Use advanced recording settings and select `FFMPEG VAAPI` encoder.
