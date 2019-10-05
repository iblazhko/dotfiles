# dotfiles

Scripts and configurations for setting up my Ubuntu Budgie environment.

## Install log

Download Ubuntu Budgie from <https://ubuntubudgie.org/>.
Use <https://www.balena.io/etcher/> to make bootable USB drive.

It is recommended to install latest LTS version. At the moment of writing LTS version is `18.04.3`.

During installation enable disk encryption.

### Prerequisites

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
 firefox \
 -y
 ```

### Oh My ZSH

```bash
sudo apt install -y zsh
sudo apt install -y git
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### Papirus Icons

```bash
sudo add-apt-repository ppa:papirus/papirus
sudo apt update
sudo apt install -y papirus-icon-theme

sudo apt install -y gnome-tweaks
```

### Bitwarden

```bash
cd ~/Downloads
wget https://vault.bitwarden.com/download/?app=desktop&platform=linux&variant=deb
mv 'index.html?app=desktop&platform=linux&variant=deb' bitwarden.deb
sudo dpkg -i bitwarden.deb
sudo apt --fix-broken install -y
rm bitwarden.deb
```

### Firefox

- enable FireFox sync
- set DuckDuckGo as default search engine

### SSH Key

Generate SSH key:

```bash
ssh-keygen -f ~/.ssh/ib_thinkpad -t rsa -b 4096 -c "<email>"
echo "Public key:"
cat ~/.ssh/ib_thinkpad.pub
```

#### GitHub

- Sign in to <https://github.com>
- At <https://github.com/settings/keys>, upload the key above

### VisualStudio Cod:

- <https://code.visualstudio.com/>
- Download `.deb`, install

### Sublime Text, Sublime Merge:

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime.list
sudo apt update
sudo apt install -y sublime-text sublime-merge
```

### BeyondCompare

- <https://scootersoftware.com/download.php>, <https://scootersoftware.com/bcompare-4.2.10.23938_amd64.deb>

### Docker

```bash
sudo apt remove -y docker docker-engine docker.io containerd runc

sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose

sudo systemctl enable docker

# sudo groupadd docker
sudo usermod -aG docker $USER
```

Re-login, then verify the installation:

```bash
docker run --rm hello-world
```

### .NET Core SDK, PowerShell:

```bash
wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb


sudo add-apt-repository universe
sudo apt update
sudo apt install -y apt-transport-https
sudo apt update
sudo apt install -y dotnet-sdk-3.0 powershell
```

Verify the installation:

```bash
dotnet --info
```

### Haskell:

```bash
curl -sSL https://get.haskellstack.org/ | sh
```

### Erlang+Elixir

```bash
sudo apt install -y erlang elixir
```

### NodeJS+NPM+Yarn

```bash
sudo apt install -y nodejs npm yarn
```

### JetBrains Rider:

- download from <https://www.jetbrains.com/rider/download>
- unpack and move to `/opt/jetbrains_rider`

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

### Slack:

Download: <https://slack.com/intl/en-gb/downloads/linux>

### Notion:

Install `nativefier`:

```bash
sudo npm install nativefier -g
```

Create electron wrapper:

```bash
nativefier --name Notion https://www.notion.so
sudo mv notion-linux-x64 /opt/notion
```

### Todoist

```bash
nativefier --name Todoist https://todoist.com/app
sudo mv todoist-linux-x64 /opt/todoist
```

### Graphics and Photography

```bash
sudo apt install -y darktable rapid-photo-downloader libmediainfo0v5 gimp dispcalgui hugin 
sudo apt install -y clinfo
```

### MailSpring

Download: <https://updates.getmailspring.com/download?platform=linuxDeb>

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
