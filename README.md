# dotfiles

Scripts and configurations for setting up my Linux environment.

## Operating system installation

Download Pop!_OS from <https://system76.com/pop>. Select appropriate version - NVIDIA vs Intel/AMD. 

To make bootable USB drive use <https://rufus.ie/> (Windows-only) or <https://www.balena.io/etcher/>.

At the moment of writing Pop!_OS version is `19.10`.

During installation enable disk encryption.

### UI  Tweaks

```bash
sudo apt install gnome-tweaks
```

#### GNOME Extensions

<https://extensions.gnome.org>

- [Caffeine](https://extensions.gnome.org/extension/517/caffeine/)
- [Clipboard Indicator](https://extensions.gnome.org/extension/779/clipboard-indicator/)
- [cpufreq](https://extensions.gnome.org/extension/1082/cpufreq/)
- [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock/)
- [Dynamic Panel Transparency](https://extensions.gnome.org/extension/1011/dynamic-panel-transparency/)
- [Topicons Plus](https://extensions.gnome.org/extension/1031/topicons/)
- [Vitals](https://extensions.gnome.org/extension/1460/vitals/)

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
 vim \
 -y
 ```

### Dependencies from LTS

Some software packages require `libssl1.0.0` and `libicu60`

```bash
wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl1.0/libssl1.0.0_1.0.2n-1ubuntu6.2_amd64.deb && \
  sudo dpkg -i libssl1.0.0_1.0.2n-1ubuntu6.2_amd64.deb

wget http://de.archive.ubuntu.com/ubuntu/pool/main/i/icu/libicu60_60.2-3ubuntu3_amd64.deb && \
  sudo dpkg -i libicu60_60.2-3ubuntu3_amd64.deb
```

### Flatpak

```bash
sudo apt install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak update
```

### OpenCL drivers

NVIDIA proprietary driver comes with OpenCL support. Install additional drivers
if required.

Check OpenCL status

```bash
sudo apt install clinfo
clinfo
```

```plaintext
Number of platforms                               1
  Platform Name                                   NVIDIA CUDA
...
  Platform Name                                   NVIDIA CUDA
Number of devices                                 1
  Device Name                                     GeForce GTX 1060 6GB
...
```

#### Intel OpenCL

Install OpenCL drivers for integrated Intel video card.

<https://github.com/intel/compute-runtime>

Follow instructions in the latest release.

#### AMD OpenCL

Install OpenCL drivers for external AMD video card.

Download drivers package from <https://www.amd.com/en/support/>.

Download link for AMD RX 580: <https://www.amd.com/en/support/graphics/radeon-500-series/radeon-rx-500-series/radeon-rx-580>

Uncompress the package.

From the drivers directory, run

```bash
./amdgpu-install --opencl=pal,legacy --headless --no-dkms
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

## Software

### Bitwarden

<https://flathub.org/apps/details/com.bitwarden.desktop>

```bash
flatpak install com.bitwarden.desktop
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
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
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
sudo ./dotnet-install.sh --version 2.2.207 --install-dir /opt/dotnet
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

Alternatively, grab packages from [PowerShell Core 6.2.3](https://github.com/PowerShell/PowerShell/releases/tag/v6.2.3) release.


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

### NodeJS+NPM

```bash
wget https://nodejs.org/dist/v12.13.0/node-v12.13.0-linux-x64.tar.xz
tar xvf node-v12.13.0-linux-x64.tar.xz

sudo mv node-v12.13.0-linux-x64 /opt/nodejs
echo 'export PATH="$PATH:/opt/nodejs/bin"' | sudo tee /etc/profile.d/nodejs-path.sh
```

### Postman+Newman

Postman:

<https://flathub.org/apps/details/com.getpostman.Postman>

```bash
flatpak install com.getpostman.Postman
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
sudo apt update
sudo apt install ca-certificates curl apt-transport-https lsb-release gnupg

curl -sL https://packages.microsoft.com/keys/microsoft.asc | 
    gpg --dearmor | 
    sudo tee /etc/apt/trusted.gpg.d/microsoft.asc.gpg > /dev/null

AZ_REPO=bionic
echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | 
    sudo tee /etc/apt/sources.list.d/azure-cli.list

sudo apt update
sudo apt install azure-cli
```

Initialize:

```bash
az login
```

Add Devops extension:

```bash
az extension add --name azure-devops
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
wget https://github.com/IBM/plex/releases/download/v4.0.2/OpenType.zip
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

<https://flathub.org/apps/details/org.remmina.Remmina>

```bash
flatpak install org.remmina.Remmina
```

### Slack

Download: <https://slack.com/intl/en-gb/downloads/linux>

### Graphics and Photography

#### DisplayCal

Ubuntu has `dispcalgui` package built-in, but it is an older version.

Download latest from <https://displaycal.net/>, install.

#### RawTherapee

```bash
flatpak install com.rawtherapee.RawTherapee
```

#### GIMP

```bash
flatpak install org.gimp.GIMP
```

#### Rapid Photo Downloader

```bash
sudo apt install -y rapid-photo-downloader
```

### Joplin

```bash
wget -O - https://raw.githubusercontent.com/laurent22/joplin/master/Joplin_install_and_update.sh | bash
```

### EverDo
<https://everdo.net/>

### OBS Studio

```bash
flatpak install com.obsproject.Studio
```

### Audacity

```bash
flatpak install org.audacityteam.Audacity
```

### Kdenlive

```bash
flatpak install kdenlive
```

### Calible

```bash
flatpak install calible
```

### Calible

```bash
flatpak install calible
```

### MarkText

```bash
flatpak install marktext
```

### Green With Envy

```bash
flatpak install com.leinardi.gwe
```

### Stacer

<https://github.com/oguzhaninan/Stacer>

### Typora

<https://typora.io/>

## Optional / Considerations

Following are links for software that is not needed in Pop_!_OS or I may consider using in the future.

### Erlang+Elixir

<https://elixir-lang.org/install.html>

```bash
wget https://packages.erlang-solutions.com/erlang-solutions_2.0_all.deb
sudo dpkg -i erlang-solutions_2.0_all.deb
rm erlang-solutions_2.0_all.deb

sudo apt update
sudo apt install esl-erlang elixir
```

### R

- <https://www.r-project.org/>
- <https://linuxize.com/post/how-to-install-r-on-ubuntu-18-04/>
- <https://www.digitalocean.com/community/tutorials/how-to-install-r-on-ubuntu-18-04>

```bash
sudo apt install apt-transport-https software-properties-common

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/'

sudo apt install r-base
```

### Pulumi

<https://www.pulumi.com/docs/get-started/install/>

```bash
curl -fsSL https://get.pulumi.com | sh
```

### MailSpring

Download: <https://updates.getmailspring.com/download?platform=linuxDeb>

### MineTime

MineTime calendar: <https://minetime.ai/>

```bash
wget https://minetime-deploy.herokuapp.com/download/linux_deb_64 -O minetime.deb
sudo dpkg -i minetime.deb
rm minetime.deb
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
