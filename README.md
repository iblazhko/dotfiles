# dotfiles

Scripts and configurations for setting up my Linux environment.

## Operating system installation

Download Elementary OS from <https://elementary.io>.
To make bootable USB drive use <https://rufus.ie/> (Windows-only) or <https://www.balena.io/etcher/> on macOS / Linux.

At the moment of writing ElementaryOS version is `5.1 Hera`.

During installation enable disk encryption.

### Oh My ZSH

<https://ohmyz.sh/>

```bash
sudo apt install -y zsh git
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

cat ./.zshrc > ~/.zshrc
```

### SSH Key

Generate SSH key:

```bash
ssh-keygen -f ~/.ssh/some_name -t rsa -b 4096 -C "some_email"
echo "Public key:"
cat ~/.ssh/some_name.pub
```

### Elementary Tweaks

<https://github.com/elementary-tweaks/elementary-tweaks>

```bash
sudo apt install software-properties-common
sudo add-apt-repository ppa:philip.scott/elementary-tweaks
sudo apt install elementary-tweaks
```

### `sysctl` Tweaks

```bash
echo """
vm.swappiness=10
vm.vfs_cache_pressure=50

fs.inotify.max_user_watches=1048576
fs.inotify.max_user_instances=256
""" | sudo tee --append /etc/sysctl.conf

sudo /sbin/sysctl -p
```

## Software

### APT Software

```bash
sudo apt update
sudo apt upgrade

sudo apt install \
 build-essential \
 tlp \
 powertop \
 cpufrequtils \
 sysstat \
 nmon \
 glances \
 psutils \
 curl \
 file \
 git \
 htop \
 httpie \
 p7zip \
 vim \
 wget \
 clinfo \
 rapid-photo-downloader \
 graphviz \
 openjdk-11-jre-headless \
 -y
 ```

### ElementaryOS AppCenter Software

- AppEditor
- Chromium Browser
- Contacts
- Cyfrif
- DConf editor
- Eddy
- Firefox Browser
- Fondo
- Formatter
- Image Burner
- Monitor
- Palaura (dictionary)
- Screen Recorder

### Flatpak Software

- Bitwarden: <https://flathub.org/apps/details/com.bitwarden.desktop>
- Calibre: <https://flathub.org/apps/details/com.calibre_ebook.calibre>
- Postman: <https://flathub.org/apps/details/com.getpostman.Postman>
- Marktext: <https://flathub.org/apps/details/com.github.marktext.marktext>
- Green With Envy: <https://flathub.org/apps/details/com.leinardi.gwe>
- OBS: <https://flathub.org/apps/details/com.obsproject.Studio>
- RawTherapee: <https://flathub.org/apps/details/com.rawtherapee.RawTherapee>
- Audacity: <https://flathub.org/apps/details/org.audacityteam.Audacity>
- GIMP: <https://flathub.org/apps/details/org.gimp.GIMP>
- Kdenlive: <https://flathub.org/apps/details/org.kde.kdenlive>
- LibreOffice: <https://flathub.org/apps/details/org.libreoffice.LibreOffice>
- Remmina: <https://flathub.org/apps/details/org.remmina.Remmina>
- Cawbird: <https://flathub.org/apps/details/uk.co.ibboard.cawbird>

- DarkTable: <https://flathub.org/apps/details/org.darktable.Darktable> **TODO:** Wait until 3.0 binaries are available.

### DEBs Software

- VisualStudio Code: <https://code.visualstudio.com/>
- Beyond Compare: <https://scootersoftware.com>
- DisplayCal: <https://displaycal.net/>
- Everdo Desktop: <https://everdo.us15.list-manage.com/track/click?u=b1bbf304b9fe6c4945443597b&id=01cdfac3d3&e=6c3ec5f512>
- Slack: <https://slack.com/intl/en-gb/downloads/linux>
- Zoom: <https://zoom.us/download?os=linux>
- Viber: <https://www.viber.com/download/>
- Skype: <https://www.skype.com/en/get-skype/>
- MullvadVPN: <https://mullvad.net/en/>
- Kubefwd: <https://github.com/txn2/kubefwd/releases>

### Brave Browser

<https://brave.com/download/>
<https://brave-browser.readthedocs.io/en/latest/installing-brave.html#linux>

```bash
sudo apt install apt-transport-https curl
curl -s https://brave-browser-apt-release.s3.brave.com/brave-core.asc | sudo apt-key --keyring /etc/apt/trusted.gpg.d/brave-browser-release.gpg add -
echo "deb [arch=amd64] https://brave-browser-apt-release.s3.brave.com/ stable main" | sudo tee /etc/apt/sources.list.d/brave-browser-release.list
sudo apt update
sudo apt install brave-browser
```

### Firefox Browser

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

### VisualStudio Code Settings

```bash
mkdir -p ~/.config/Code/User
cp .config/Code/User/settings.json ~/.config/Code/User/
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

sudo ./dotnet-install.sh --version 2.2.207 --install-dir /opt/dotnet
sudo ./dotnet-install.sh --version 3.1.101 --install-dir /opt/dotnet

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

### NodeJS+NPM

```bash
wget https://nodejs.org/dist/v12.13.0/node-v12.13.0-linux-x64.tar.xz
tar xvf node-v12.13.0-linux-x64.tar.xz

sudo mv node-v12.13.0-linux-x64 /opt/nodejs
echo 'export PATH="$PATH:/opt/nodejs/bin"' | sudo tee /etc/profile.d/nodejs-path.sh
```

### Newman

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

#### `kubectx`/`kubens`

```bash
cd ~/Downloads
git clone https://github.com/ahmetb/kubectx.git

mkdir ~/.local/bin
cp kubectx/kubectx kubectx/kubens  ~/.local/bin
rm -rf kubectx
```

#### Helm

<https://github.com/helm/helm/releases>

Download release, uncompress, move `helm` to `~/.local.bin`:

```bash
mv linux-amd64/helm ~/.local/bin
```

### JetBrains IDEs

- Rider: <https://www.jetbrains.com/rider/download> -> `/opt/jetbrains_rider`
- PyCharm: <https://www.jetbrains.com/pycharm/download/> -> `/opt/jetbrains_pycharm`
- IDEA: <https://www.jetbrains.com/idea/download> -> `/opt/jetbrains_idea`
- DataGrip: <https://www.jetbrains.com/datagrip/download> -> `/opt/jetbrains_datagrip`

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

Download PragmataPro fonts (link in the email).

... extract all of the above

```bash
mkdir ~/.local/share/fonts

mv OpenType ~/.local/share/fonts/IBM_Plex
mv Input-Font/Input_Fonts ~/.local/share/fonts/
mv FiraCode_2/otf ~/.local/share/fonts/FiraCode
mv source-code-pro-2.030R-ro-1.050R-it/OTF ~/.local/share/fonts/SourceCode
mv source-sans-pro-3.006R/OTF ~/.local/share/fonts/SourceSans
mv source-serif-pro-3.000R/OTF ~/.local/share/fonts/SourceSerif
mv PragmataPro ~/.local/share/fonts/PragmataPro

fc-cache -f -r -v
```

### Joplin

```bash
wget -O - https://raw.githubusercontent.com/laurent22/joplin/master/Joplin_install_and_update.sh | bash
```

### Typora
<https://typora.io/>

```bash
# or run:
# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -

# add Typora's repository
sudo add-apt-repository 'deb https://typora.io/linux ./'
sudo apt-get update

# install typora
sudo apt-get install typora
```

## Optional / Considerations

Following are links for software that is not needed in Elementary or I may consider using in the future.

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


### Mount Windows Shares

Example of using a Windows file share:

```bash
sudo apt install -y cifs-utils
sudo mount -t cifs //192.168.1.172/data /mnt/win-data -o user=testuser
```

### Video drivers

#### NVIDIA

In "Software & Updates" settings / "Additional Drivers" select latest NVIDIA
driver package; apply; reboot.

This will install both graphics and OpenCL drivers.

At the moment of writing NVIDIA driver package is `nvidia-driver-435`.

#### Intel OpenCL

Install OpenCL drivers for integrated Intel video card.

<https://github.com/intel/compute-runtime>

Follow instructions in the latest release.

#### AMD OpenCL

Install OpenCL drivers for external AMD video card.

Download drivers package from <https://www.amd.com/en/support/>.

Download link for AMD RX 580: <https://www.amd.com/en/support/graphics/radeon-500-series/radeon-rx-500-series/radeon-rx-580>

Uncompress the package.

In the `amdgpu-install` script add `elementary` to the list of supported distributions (around L134)

```bash
function os_release() {
  if [[ -r  /etc/os-release ]]; then
    . /etc/os-release

  case "$ID" in
    ubuntu|linuxmint|debian|elementary)
      :
      ;;
```

From the drivers directory, run

```bash
./amdgpu-install -y --opencl=pal,legacy --headless --no-dkms
```

### App Indicators

<https://www.linuxuprising.com/2018/08/how-to-re-enable-ayatana-appindicators.html>
<https://github.com/mdh34/elementary-indicators>

```bash
cp /etc/xdg/autostart/indicator-application.desktop ~/.config/autostart/
sed -i 's/^OnlyShowIn.*/OnlyShowIn=Unity;GNOME;Pantheon;/' ~/.config/autostart/indicator-application.desktop
```
