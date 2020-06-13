# dotfiles

Scripts and configurations for setting up my Linux environment.

## Operating system installation

Download Elementary OS from <https://elementary.io>.
To make a bootable USB drive use <https://rufus.ie/> (Windows-only) or <https://www.balena.io/etcher/> (macOS/Linux/Windows).

At the moment of writing ElementaryOS version is `5.1.5 Hera`.

During installation enable disk encryption.

### Oh My ZSH

<https://ohmyz.sh/>

```sh
sudo apt install -y zsh git
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

cat ./.zshrc | tee ~/.zshrc
```

### SSH Key

Generate SSH key:

```sh
ssh-keygen -f ~/.ssh/$(hostname) -t rsa -b 4096 -C "$(hostname)"
echo "Public key:"
cat ~/.ssh/$(hostname).pub
```

### Elementary Tweaks

<https://github.com/elementary-tweaks/elementary-tweaks>

```sh
sudo apt install software-properties-common -y
sudo add-apt-repository -y ppa:philip.scott/elementary-tweaks
sudo apt install elementary-tweaks -y
```

- "Files": disable single click
- "Terminal": set solid background color
- "Terminal": disable "Natural copy paste"
- Adjust fonts settings (once the fonts below installed):
  - Default font: `Fira Sans Regular 11`
  - Document font: `Fira Sans Regular 12`
  - Monospace font: `Input Mono Regular 12`
  - Titlebar font: `Fira Sans SemiBold 12`

### Plank Tweaks

```sh
mkdir -p ~/.local/share/plank/themes
cp -R ./.local/share/plank/themes/Mac ~/.local/share/plank/themes
```

Set Plank theme to `Mac`.

### `sysctl` Tweaks

```sh
echo """
vm.swappiness=10
vm.vfs_cache_pressure=50

fs.inotify.max_user_watches=1048576
fs.inotify.max_user_instances=1024
""" | sudo tee --append /etc/sysctl.conf

sudo /sbin/sysctl -p
```

### Power Management

#### PowerTop

Calibration may take 15-20min and will turn components on/off (including display and wireless).

```sh
sudo powertop --calibrate
```

After running `powertop` for some time on both AC and battery:

```sh
sudo powertop --auto-tune
```

#### Undervolting

- <https://github.com/kitsunyan/intel-undervolt>
- <https://github.com/georgewhewell/undervolt>

### Application Indicators

- <https://elementaryos.stackexchange.com/questions/17452/how-to-display-system-tray-icons-in-elementary-os-juno>
- <https://elementaryos.stackexchange.com/questions/21547/double-wi-fi-icon-after-installing-wingpanel-indicator-ayatana>

```sh
sudo add-apt-repository -y ppa:yunnxx/elementary
sudo apt update
sudo apt install -y indicator-application wingpanel-indicator-ayatana
sudo sed -i -e 's/OnlyShowIn=Unity;GNOME;/OnlyShowIn=Unity;GNOME;Pantheon;/g' /etc/xdg/autostart/indicator-application.desktop
sudo mv /etc/xdg/autostart/nm-applet.desktop /etc/xdg/autostart/nm-applet.orig
```

Log out and log back in, or

```sh
sudo service lightdm restart
```

### Video drivers

#### NVIDIA

In "AppCenter" / "Drivers" select latest NVIDIA driver package; apply; reboot.

Alternatively, install explicitly:

```sh
sudo apt install -y nvidia-driver-440
```

This will install both graphics and OpenCL drivers.

At the moment of writing NVIDIA driver package is `nvidia-driver-440`.

##### Screen Tearing

<https://github.com/Askannz/nvidia-force-comp-pipeline>

Enable `Force Composition Pipeline` on all monitors connected to an Nvidia card as a workaround against screen tearing:

```sh
echo """
[Desktop Entry]
Type=Application
Name=Force Composition Pipeline (NVIDIA)
Comment=Simple script to enable 'Force Composition Pipeline' on all monitors connected to an NVIDIA card as a workaround against screen tearing.
Exec=/usr/local/bin/nvidia-force-comp-pipeline
""" | sudo tee /etc/xdg/autostart/nvidia-settings-force-comp-pipeline.desktop

git clone https://github.com/Askannz/nvidia-force-comp-pipeline
sudo cp nvidia-force-comp-pipeline/nvidia-force-comp-pipeline /usr/local/bin/
rm -rf nvidia-force-comp-pipeline
```

#### Intel OpenCL

Install OpenCL drivers for integrated Intel video card.

<https://github.com/intel/compute-runtime>

Follow instructions in the latest release.

## Software

### APT Software

```sh
sudo apt update
sudo apt install \
 build-essential \
 git \
 vim \
 p7zip \
 chromium-browser \
 firefox \
 curl \
 wget \
 httpie \
 file \
 cifs-utils \
 cpufrequtils \
 glances \
 htop \
 neofetch \
 nmon \
 powertop \
 psensor \
 psutils \
 sysstat \
 tlp \
 dconf-editor \
 graphviz \
 openjdk-11-jre-headless \
 rapid-photo-downloader \
 mediainfo \
 clinfo \
 -y
```

### ElementaryOS AppCenter Software

- AppEditor
- Contacts
- Cyfrif
- Eddy
- Fondo
- Monitor
- Palaura (dictionary)
- Screen Recorder
- Webpin
- Optimizer

```sh
sudo apt install \
  com.github.donadigo.appeditor \
  com.github.aggalex.contacts \
  com.github.aggalex.contacts \
  com.github.donadigo.eddy \
  com.github.calo001.fondo \
  com.github.stsdc.monitor \
  com.github.lainsce.palaura \
  com.github.mohelm97.screenrecorder \
  com.github.artemanufrij.webpin \
  com.github.hannesschulze.optimizer \
  -y
```

### Flatpak Software

- Bitwarden: <https://flathub.org/apps/details/com.bitwarden.desktop>
- Calibre: <https://flathub.org/apps/details/com.calibre_ebook.calibre>
- Postman: <https://flathub.org/apps/details/com.getpostman.Postman>
- Marktext: <https://flathub.org/apps/details/com.github.marktext.marktext>
- Green With Envy: <https://flathub.org/apps/details/com.leinardi.gwe>
- OBS Studio: <https://flathub.org/apps/details/com.obsproject.Studio>
- Audacity: <https://flathub.org/apps/details/org.audacityteam.Audacity>
- GIMP: <https://flathub.org/apps/details/org.gimp.GIMP>
- Kdenlive: <https://flathub.org/apps/details/org.kde.kdenlive>
- Foliate: <https://flathub.org/apps/details/com.github.johnfactotum.Foliate>
- LibreOffice: <https://flathub.org/apps/details/org.libreoffice.LibreOffice>
- Remmina: <https://flathub.org/apps/details/org.remmina.Remmina>
- Cawbird: <https://flathub.org/apps/details/uk.co.ibboard.cawbird>
- Planner: <https://flathub.org/apps/details/com.github.alainm23.planner>
- Slack: <https://flathub.org/apps/details/com.slack.Slack>
- Zoom: <https://flathub.org/apps/details/us.zoom.Zoom>
- Skype: <https://flathub.org/apps/details/com.skype.Client>
- Viber: <https://flathub.org/apps/details/com.viber.Viber>
- _DarkTable_: <https://flathub.org/apps/details/org.darktable.Darktable> (Flatpak not fully compatible with Plank)

```sh
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

flatpak install flathub \
  com.bitwarden.desktop \
  com.calibre_ebook.calibre \
  com.getpostman.Postman \
  com.github.alainm23.planner \
  com.github.johnfactotum.Foliate \
  com.github.marktext.marktext \
  com.leinardi.gwe \
  com.obsproject.Studio \
  com.skype.Client \
  com.slack.Slack \
  com.viber.Viber \
  org.audacityteam.Audacity \
  org.gimp.GIMP \
  org.kde.kdenlive \
  org.libreoffice.LibreOffice \
  org.remmina.Remmina \
  uk.co.ibboard.cawbird \
  us.zoom.Zoom \
  -y
```

### DEBs Software

- VisualStudio Code: <https://code.visualstudio.com/>
- Beyond Compare: <https://scootersoftware.com>
- Darktable: <https://software.opensuse.org/download.html?project=graphics:darktable&package=darktable>
- DisplayCal: <https://displaycal.net/>
- MullvadVPN: <https://mullvad.net/en/>
- Kubefwd: <https://github.com/txn2/kubefwd/releases>

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

### Ungoogled Chromium

<https://github.com/Eloston/ungoogled-chromium>
<https://software.opensuse.org/download/package?package=ungoogled-chromium&project=home:ungoogled_chromium>

### GitHub

- Sign in to <https://github.com>
- At <https://github.com/settings/keys>, upload the SSH key created above

### VisualStudio Code Settings

```sh
mkdir -p ~/.config/Code/User
cp .config/Code/User/settings.json ~/.config/Code/User/
```

### Sublime Text, Sublime Merge

- <https://www.sublimetext.com>
- <https://www.sublimemerge.com>

```sh
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime.list
sudo apt update
sudo apt install -y sublime-text sublime-merge
```

```sh
mkdir -p ~/.config/sublime-text-3/Packages/User
cp .config/sublime-text-3/Packages/User/Preferences.sublime-settings ~/.config/sublime-text-3/Packages/User
```

### Docker

- <https://docs.docker.com/install/linux/docker-ce/ubuntu>
- <https://docs.docker.com/install/linux/linux-postinstall>

```sh
# sudo apt remove -y docker docker-engine docker-compose docker.io containerd runc

sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository -y "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose

sudo systemctl enable docker
sudo usermod -aG docker $USER
```

Re-login, then verify the installation:

```sh
docker run --rm hello-world
```

#### CTop

<https://ctop.sh/>

```sh
mkdir -p ~/.local/bin
wget https://github.com/bcicen/ctop/releases/download/v0.7.3/ctop-0.7.3-linux-amd64 -O ~/.local/bin/ctop
chmod +x ~/.local/bin/ctop
```

### .NET Core SDK

<https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-install-script>

```sh
sudo apt install -y apt-transport-https

wget https://dot.net/v1/dotnet-install.sh
chmod +x ./dotnet-install.sh

sudo ./dotnet-install.sh --version 3.1.105 --install-dir /opt/dotnet
sudo ./dotnet-install.sh --version 3.1.301 --install-dir /opt/dotnet

echo """
export PATH="\$PATH:/opt/dotnet:\$HOME/.dotnet/tools"
export DOTNET_ROOT=/opt/dotnet
export DOTNET_CLI_TELEMETRY_OPTOUT=1
""" | sudo tee /etc/profile.d/dotnet.sh

source /etc/profile.d/dotnet.sh
```

Verify the installation:

```sh
dotnet --info
```

### PowerShell

<https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-7>

```sh
wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo apt install -y ./packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

sudo apt update
sudo apt install -y powershell
```

### NodeJS+NPM

```sh
wget https://nodejs.org/dist/v12.18.0/node-v12.18.0-linux-x64.tar.xz
tar xvf node-v12.18.0-linux-x64.tar.xz

sudo mv node-v12.18.0-linux-x64 /opt/nodejs
echo 'export PATH="$PATH:/opt/nodejs/bin"' | sudo tee /etc/profile.d/nodejs.sh
```

### Newman

```sh
sudo -i
source /etc/profile.d/nodejs.sh
npm install -g newman
```

### Python

```sh
sudo apt install -y python3-venv python3-pip python3.8 python3.8-dev python3.8-venv
```

For `wxPython` support:

```sh
sudo apt install -y libgtk-3-dev
```

### Azure CLI

<https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-apt?view=azure-cli-latest>

```sh
sudo apt update
sudo apt install ca-certificates curl apt-transport-https lsb-release gnupg

curl -sL https://packages.microsoft.com/keys/microsoft.asc |
    gpg --dearmor |
    sudo tee /etc/apt/trusted.gpg.d/microsoft.asc.gpg > /dev/null

AZ_REPO=bionic
echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" |
    sudo tee /etc/apt/sources.list.d/azure-cli.list

sudo apt update
sudo apt install -y azure-cli
```

Initialize:

```sh
az login
```

Add Devops extension:

```sh
az extension add --name azure-devops
```

### Google Cloud Platform SDK

<https://cloud.google.com/sdk/>
<https://cloud.google.com/sdk/docs/quickstart-debian-ubuntu>

```sh
# Add the Cloud SDK distribution URI as a package source
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

# Import the Google Cloud Platform public key
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -

# Update the package list and install the Cloud SDK
sudo apt update && sudo apt install -y google-cloud-sdk kubectl
```

Initialize:

```sh
gcloud init
```

After logged in, navigate to <https://console.cloud.google.com/kubernetes/list>, select cluster and view command line to connect:

```sh
gcloud container clusters get-credentials dev-cluster --zone <your-zone> --project <your-project>
```

#### `kubectx`/`kubens`

```sh
cd ~/Downloads
git clone https://github.com/ahmetb/kubectx.git

mkdir -p ~/.local/bin
cp kubectx/kubectx kubectx/kubens  ~/.local/bin
rm -rf kubectx
```

#### Helm

<https://github.com/helm/helm/releases>

Download release, uncompress, move `helm` to `~/.local.bin`:

```sh
mv linux-amd64/helm ~/.local/bin
```

### Virtualization

- KVM
- QEMU
- Libvitr
- Virt-Manager

Check compatibility:

```sh
LC_ALL=C lscpu | grep Virtualization
lsmod | grep kvm
```

```sh
sudo apt install \
  qemu qemu-kvm \
  libvirt-bin \
  bridge-utils \
  virtinst \
  virt-manager
  -y

sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
```

### JetBrains IDEs

- Rider: <https://www.jetbrains.com/rider/download> -> `/opt/jetbrains_rider`
- PyCharm: <https://www.jetbrains.com/pycharm/download/> -> `/opt/jetbrains_pycharm`
- IDEA: <https://www.jetbrains.com/idea/download> -> `/opt/jetbrains_idea`
- DataGrip: <https://www.jetbrains.com/datagrip/download> -> `/opt/jetbrains_datagrip`

### Fonts

Download Input fonts: <https://input.fontbureau.com/download/>

Download IBM Plex: <https://www.ibm.com/plex/>

```sh
wget https://github.com/IBM/plex/releases/download/v4.0.2/OpenType.zip
```

Download Fira Sans: <https://www.fontsquirrel.com/fonts/fira-sans>
Download Fira Code: <https://github.com/tonsky/FiraCode>

Download Adobe Source fonts:

```sh
wget https://github.com/adobe-fonts/source-code-pro/releases/download/2.030R-ro%2F1.050R-it/source-code-pro-2.030R-ro-1.050R-it.zip
wget https://github.com/adobe-fonts/source-serif-pro/releases/download/3.000R/source-serif-pro-3.000R.zip
wget https://github.com/adobe-fonts/source-sans-pro/releases/download/3.006R/source-sans-pro-3.006R.zip
```

Download PragmataPro fonts (link in the email).

... extract all of the above

```sh
mkdir ~/.local/share/fonts

mv OpenType ~/.local/share/fonts/IBM_Plex
mv Input-Font/Input_Fonts ~/.local/share/fonts/
mv fira-sans ~/.local/share/fonts/FiraSans
mv FiraCode_3.1/otf ~/.local/share/fonts/FiraCode
mv source-code-pro-2.030R-ro-1.050R-it/OTF ~/.local/share/fonts/SourceCode
mv source-sans-pro-3.006R/OTF ~/.local/share/fonts/SourceSans
mv source-serif-pro-3.000R/OTF ~/.local/share/fonts/SourceSerif
mv PragmataPro ~/.local/share/fonts/PragmataPro

fc-cache -f -r -v
```

## Todoist

<https://github.com/KryDos/todoist-linux/releases>

Alternatively, use `Webpin` to create <https://todoist.com/app> webapp shortcut.

### Notion

Use `Webpin` to create <https://notion.so> webapp shortcut.
Use icon `icons/notion.png`.

### SimpleNote

<https://github.com/Automattic/simplenote-electron/releases>

Alternatively, use `Webpin` to create <https://app.simplenote.com/> webapp shortcut.

### GeekBench

<https://www.geekbench.com/download/>

### Balena Etcher

<https://www.balena.io/etcher/>

```sh
wget https://github.com/balena-io/etcher/releases/download/v1.5.89/balenaEtcher-1.5.89-x64.AppImage
chmod +x balenaEtcher-1.5.89-x64.AppImage
sudo mv balenaEtcher-1.5.89-x64.AppImage /opt/balenaEtcher.AppImage

mkdir -p ~/.local/share/applications
cp ./.local/share/applications/balenaEtcher.desktop ~/.local/share/applications/balenaEtcher.desktop
cp ./icons/balenaEtcher.png ~/Pictures/icons/balenaEtcher.png
```

## Optional / Considerations

Following are links for software that is not needed in Elementary or I may consider using in the future.

### Joplin

```sh
wget -O - https://raw.githubusercontent.com/laurent22/joplin/master/Joplin_install_and_update.sh | bash
```

### Rust

- <https://www.rust-lang.org>
- <https://www.rust-lang.org/tools/install>

```sh
curl https://sh.rustup.rs -sSf | sh
```

### Haskell

<https://docs.haskellstack.org>

```sh
curl -sSL https://get.haskellstack.org/ | sh
```

### Scala/SBT

- <https://www.scala-lang.org/>
- <https://www.scala-sbt.org/download.html>

```sh
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | sudo apt-key add

sudo apt update
sudo apt install sbt
```

### Erlang+Elixir

<https://elixir-lang.org/install.html>

```sh
wget https://packages.erlang-solutions.com/erlang-solutions_2.0_all.deb
sudo dpkg -i erlang-solutions_2.0_all.deb
rm erlang-solutions_2.0_all.deb

sudo apt update
sudo apt install esl-erlang elixir
```

### R

- <https://www.r-project.org/>
- <https://www.digitalocean.com/community/tutorials/how-to-install-r-on-ubuntu-18-04>

```sh
sudo apt install apt-transport-https software-properties-common

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran40/'

sudo apt install r-base
```

### Pulumi

<https://www.pulumi.com/docs/get-started/install/>

```sh
curl -fsSL https://get.pulumi.com | sh
```

### Mount Windows Shares

Example of using a Windows file share:

```sh
sudo apt install -y cifs-utils
sudo mount -t cifs //192.168.1.172/data /mnt/win-data -o user=testuser
```

### AMD OpenCL

Install OpenCL drivers for external AMD video card.

Download drivers package from <https://www.amd.com/en/support/>. E.g. for AMD RX 580: <https://www.amd.com/en/support/graphics/radeon-500-series/radeon-rx-500-series/radeon-rx-580>

Uncompress the package.

In the `amdgpu-install` script add `elementary` to the list of supported distributions (around L134)

```sh
function os_release() {
  if [[ -r  /etc/os-release ]]; then
    . /etc/os-release

  case "$ID" in
    ubuntu|linuxmint|debian|elementary)
      :
      ;;
```

From the drivers directory, run

```sh
./amdgpu-install -y --opencl=pal,legacy --headless --no-dkms
```

### Stacer

<https://github.com/oguzhaninan/Stacer>

```sh
sudo add-apt-repository -y ppa:oguzhaninan/stacer
sudo apt update
sudo apt install -y stacer
```

### Typora

<[Typora — a markdown editor, markdown reader.](https://typora.io/)>

```sh
# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -
sudo add-apt-repository -y "deb https://typora.io/linux ./"
sudo apt update
sudo apt install -y typora
```

### Ulauncher

<https://ulauncher.io/>
