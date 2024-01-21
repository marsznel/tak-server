# TAK SERVER

![TAK logo](img/tak.jpg)

This is a Docker wrapper for an official 'OG' TAK server from [TAK Product Center](https://tak.gov/) intended for beginners. It will give you a turnkey TAK server with SSL which works with ATAK, iTAK, WinTAK.

The key improvements are:

 - Automatic configuration.
 - Certificate generation.
 - Secure password generation.
 - Updates PostgreSQL 10 to PostgreSQL 14.
 - Updates Debian 8 to Debian 11.

## IMPORTANT: Download the Official TAK Release

Before you can build this, you must download a **TAKSERVER-DOCKER-X.X-RELEASE**.

The scripts in this repository **have not** been checked against the **HARDENED** Docker release (`TAKSERVER-DOCKER-HARDENED-X.X-RELEASE`), so please **DO NOT** use them with that version of TAK Server.

Releases are now public at [https://tak.gov/products/tak-server](https://tak.gov/products/tak-server)

Please follow account registration process, and once completed go to the link above.

The integrity of the release will be checked at setup against the MD5/SHA1 checksums in this repo. **THESE MUST MATCH**. If they do not match, **DO NOT** proceed unless you trust the release. 

![TAK release download](img/tak-server-download.jpg)

## TAK Server Release Checksums

The size blew up after `4.6` due to `900GB` of DTED which was added to WebTAK.

| Release Filename                      | Bytes       | MD5 Checksum                       | SHA1 Checksum                              |
| ------------------------------------- | ----------- | ---------------------------------- | ------------------------------------------ |
| `takserver-docker-4.8-RELEASE-31.zip` | `772606000` | `c07f01d74960287bfc7dc08ecd6cbc3a` | `387ea4f593763d3adcfda5128a89dda4fd82e937` |
| `takserver-docker-5.0-RELEASE-29.zip` | `689840222` | `1ff2c99651ff4ab90a6edc79663b82d9` | `e2f0d957ac74577fb5ac402338bbdf3e1a16f6e2` |

## Requirements

- Debian-based operating system, such as Debian or Ubuntu
- Docker with `compose` (https://docs.docker.com/engine/install/ubuntu/ or https://docs.docker.com/engine/install/debian/)
- A TAK server release
- 8GB memory
- Network connection
- `unzip` and `netstat` utilities

## Setup Video 

Please use the below link to see a short video on a complete setup of TAK Server.

[TAK Server install (Docker)](https://www.youtube.com/watch?v=h4PA9NN-cDk)

# Steps for DigitalOcean droplet

## Add Docker's official GPG key:
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

## Add the repository to Apt sources:
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

## Install the Docker packages:
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Verify that the Docker Engine installation is successful by running the hello-world image:
```bash
sudo docker run hello-world
```

## Install Git:
```bash
sudo apt install git-all
```

## Update Debian Packages Before OpenJDK 17 Installation:
```bash
sudo apt update && sudo apt upgrade
```

## Restart the DigitalOcean server and reconnect after reboot
```bash
shutdown -r now
```

## Search for OpenJDK 17 Packages:
```bash
apt-cache search openjdk | grep openjdk-17
```

## Install the OpenJDK 17 JRE and JDK on Debian:
```bash
sudo apt install openjdk-17-jre
sudo apt install openjdk-17-jdk
```

## Confirm the Installation of OpenJDK 17:
```bash
java --version
```

## Pull GitHub repo:
```bash
sudo apt install net-tools unzip zip
git clone https://github.com/marsznel/tak-server.git
cd tak-server
```

## Paste TAKSERVER-DOCKER-X.X-RELEASE ZIP file to the tak-server directory.

## Run TAK server instalation:
```bash
chmod +x scripts/setup.sh
./scripts/setup.sh
```
## Download certs from /root/tak-server/tak/certs/files

### Docker Security

**IT IS NOT RECOMMENDED TO RUN PRIVILEGED CONTAINERS ON THE DOCKER HOST.**

These scripts assume you don't need to `sudo` for `docker` and `docker-compose`. As such it is assumed that your user account is a member of the `docker` group as is indicated during the correct installation of Docker. This will allow you to run `docker` commands without `sudo`.

For more information please consult [the official installation documentation provided by Docker](https://docs.docker.com/engine/install/linux-postinstall/).

To test if you are able to run `docker` commands without `sudo` you can test with the following command:

```bash
docker run hello-world
```

### Network Ports

TAK server needs the following port numbers to operate. Services already using these will cause a problem which the script will detect and offer a resolution for.

- `5432`
- `8089`
- `8443`
- `8444`
- `8446`
- `9000`
- `9001`

If you are going to expose these ports be careful. Not all of them run secure protocols. For peace of mind, and for working through firewalls and NAT routers run this on a VPN like OpenVPN or NordVPN.

### Successful Installation

If your TAK Server was able to successfully be installed then you should see in your console a similar message:

```console
Import the admin.p12 certificate from this folder to your browser as per the README.md file
Login at https://10.0.0.6:8443 with your admin account. No need to run the /setup step as this has been done.
Certificates and *CERT DATA PACKAGES* are in tak/certs/files 

Setup script sponsored by CloudRF.com - "The API for RF"

---------PASSWORDS----------------

Admin user name: admin
Admin password: <Your password here>
Postgresql password: <Your password here>

---------PASSWORDS----------------

MAKE A NOTE OF YOUR PASSWORDS. THEY WON'T BE SHOWN AGAIN.
Docker containers should automatically start with the docker service from now on.
```

## Admin Login

The login to the web interface requires the certificate created during setup. The certificate needs to be uploaded to the browser first. The name of this certificate is the one which you have typed after specifying the State, City, and Company during the certificate creation. 

Default certificate name is `admin.p12`. The certificates names can be checked by:

```bash
docker exec -it tak-server-tak-1 ls -hal /opt/tak/certs/files 
```

### Installing Your Admin Certificate

The `admin.p12` certificate needs to be copied from `./tak/certs/files/` and installed in a web browser for you to be able to administer your TAK Server. This not only provides TLS transport security with mutual authentication (Client > Server, Server > Client) but it proves your identity and saves you having to type a tedious password each time.


#### Google Chrome

* Go to **"Settings"** --> **"Privacy and Security"** --> **"Security"** --> **"Manage Certificates"**
* Navigate to **"Your certificates"** 
* Press **"Import"** button and choose your `.p12` file (Default password is `atakatak`)

The web UI should be now accessible via the address given below.

#### Mozilla Firefox

* Go to **"Settings"** --> **"Privacy & Security"** --> scroll down to **"Certificates"** section.
* Click the button **"View Certificates"**
* Choose **"Your Certificates"** section and **"Import"** your `.p12` certificate (Default password is `atakatak`)
* Choose the **"Authorities"** section
* Locate **"TAK"** line, there should be your certificate name displayed underneath it
* Click your certificate name and press button **"Edit Trust"**
* __*TICK*__ the box with **"This certificate can identify web sites"** statement, then click **"OK"**

The web UI should be now accessible via the address given below.

## Web UI Access

The web user interface can be only accessed via **SSL** on port **8443**.

The login prompt will not show up as the server authenticates the user based on the uploaded certificate.

The user interface is available at the below address and on all other NICs. Check your firewall as you may not want this exposed on a public NIC.

`https://localhost:8443`

### Re-Starting Server After Shutdown

Make sure you are in the main `tak-server` directory and append the `-d` flag to run the process in the background.

```bash
cd tak-server
docker compose up -d
```

### Shutting Down Running TAK Server

Make sure you are in the main `tak-server` directory.

```bash
cd tak-server
docker compose down
```

### Logging

You can access a shell in the running Docker container with this command:

```bash
docker exec -it tak-server-tak-1 tail -f /opt/tak/logs/takserver.log
```

To tail the server log from **OUTSITE** the container as the `tak` folder is mapped:

tail -f ./tak/logs/takserver.log

### Clean Up

```bash
sudo ./scripts/cleanup.sh
```

This script will stop the TAK Server container, remove the mapped database volume and remove the folder `tak` which is created in the project root directory (cloned from GitHub) during the setup process. 

**WARNING** - If you have data in an existing TAK database container it will be lost.

## Adding Your First EUD / ATAK Device

If you've never setup ATAK with a server before you need server and user certificates. You can load these manually as `.p12` files or the easier way is with a `.zip` data package and a manifest.

You can find ready made data packages in the `tak/certs/files` directory. You need to copy these to your device's SD card then import the `.zip` into ATAK / iTAK with the "Import" function and choose "Local SD".

This will add a server, certificates and a user account. You will still need to create this user with the matching name for example, `user1`, in your TAK server user management dashboard and assign them to a common group.

### Transferring Your ZIP files Via HTTP

If you like to live dangerously, you can run a script to serve the `.zip` files on TCP port `12345`, for example, http://0.0.0.0:12345. This launches a mini Python web server and serves the content of the `share` folder which will contain your certificates. Note that sharing certificates via insecure protocols is not secure. 

```console
./scripts/shareCerts.sh
Serving HTTP on 0.0.0.0 port 12345 (http://0.0.0.0:12345/) ...
10.0.0.5 - - [23/Nov/2022 15:49:52] "GET / HTTP/1.1" 200 -
10.0.0.5 - - [23/Nov/2022 15:49:54] "GET /user1-10.0.0.3.dp.zip HTTP/1.1" 200 
```

Stop the script with `Ctrl-C` once done to stop randoms fetching your certificates.

# FAQ

See [Frequently Asked Questions](FAQ.md).

## Contributing

Please feel free to open merge requests. A beginner's guide to GitHub.com is here:

https://www.freecodecamp.org/news/how-to-make-your-first-pull-request-on-github-3/

## Authors and Acknowledgment

Thanks to the TAK product center for open-sourcing and maintaining all things TAK. 

Thanks to James Wu 'wubar' on GitLab/Discord for publishing the Docker wrapper on which this was built.

Thanks to protectionist dinosaurs, on both sides of the pond, who are threatened by TAK's open source model for the motivation :p

## Useful Links

- [TAK server on TAK.gov](https://tak.gov/products/tak-server)
- [ATAK-CIV on Google Play](https://play.google.com/store/apps/details?id=com.atakmap.app.civ&hl=en_GB&gl=US)
- [iTak on Apple App store](https://apps.apple.com/my/app/itak/id1561656396)
- [WinTAK-CIV on TAK.gov](https://tak.gov/products/wintak-civ)
