<div align="center">

```
    ██╗ ██╗ ██╗
   ██╔╝████████╗
  ██╔╝ ╚██╔═██╔╝
 ██╔╝  ████████╗
██╔╝   ╚██╔═██╔╝
╚═╝     ╚═╝ ╚═╝
```
</div>

## Requirements

Before starting the installation script, please go over all the necessary requirements.

### Host where the environment will be installed

* Host minimum hardware specification
    * x86_64 architecture Linux OS
    * min 4 vcpu
    * min 16GB RAM
    * min 200GB disk
* The host needs to have public IP and TCP ports 80, 443, and 30000 opened (also TCP 6443 if you want to access the Kubernetes cluster from your local machine)
* The script has been currently tested on Debian-based distros (Ubuntu/Debian)

### Valid domain
Registered domain with base domain and wildcard pointed to your host IP where
* domain name IN A host.ip
* *.domain-name IN A host.ip

### OAuth App created with one of the Identity providers
One of the identity provider OAuth App set:
* [Gitlab OAuth App](https://docs.gitlab.com/ee/integration/oauth_provider.html)
* [Github OAuth App](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/creating-an-oauth-app)

Values to set in the identity provider:
* Homepage URL: https://{{ domain-name }}
* Authorization callback URL: https://id.{{ domain-name }}

## Setup

```
git clone https://github.com/daytonaio/installer
cd installer
./setup.sh --install
```
You will need the next 3 values:

* `URL` - domain name you have set in your DNS provider and pointing to IP address of the machine where you are deploying Daytona
* `IDP_ID` - client ID you get from your identity provider as stated in [Requirements](#requirements)
* `IDP_SECRET` - client secret you get from your identity provider as stated in [Requirements](#requirements)
̨̨̨
After running the script, you will be prompted to input those values:
```
./setup.sh --install
...
Enter app hostname (valid domain) (URL): daytona.example.com
Enter GitHub Client ID (IDP_ID): changeme
Enter GitHub Client Secret (IDP_SECRET): changeme
```
After variables are set, the prompt will show you A records that need to be added to your DNS zone, and certbot will also show you information on how to edit your DNS zone in order to get a valid wildcard certificate, so please follow the instructions.

It is also possible to set all three values via CLI when running the script:
```
URL="daytona.example.com" IDP_ID="changeme" IDP_SECRET="changeme" ./setup.sh --install
```

## Restart/Cleanup

If you want to remove and start all over, you can run the script with the `--remove` parameter, and it will delete k3s cluster with all the tools installed. Afterwards, you can create everything again with `--install`.

```
./setup.sh --remove
```