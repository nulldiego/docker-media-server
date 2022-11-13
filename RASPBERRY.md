# Installing on Raspberry Pi

Those are just some quick notes and tips.

## Raspberry OS Lite

Install Raspberry OS Lite to SD Card using Raspberry Pi Imager.

## raspi-config

```bash
$ raspi-config
```

Utility to set up stuff like keyboard distribution, password, Wi-Fi, SSH...

## SSH

On computer from which you wish to connect to Raspberry Pi:

```bash
$ ssh-keygen
$ scp ~/.ssh/id_rsa.pub pi@<raspberry-ip>:/home/pi/uploaded_key.pub
```

Then you need to add the public key that you sent to Raspberry to its `authorized_keys`.

On Raspberry Pi:

```bash
$ mkdir .ssh
$ chmod 700 .ssh
$ touch .ssh/authorized_keys
$ chmod 600 .ssh/authorized_keys
$ echo `cat ~/uploaded_key.pub` >> ~/.ssh/authorized_keys
$ rm uploaded_key.pub
```

Add the following lines to `/etc/ssh/sshd_config` (only allow SSH for user pi and avoid password authentication):

```
AllowUsers pi
PasswordAuthentication no
```

## NO-IP

Register a host at NO-IP website, then configure Dynamic DNS on your router (probably @ http://192.168.1.1, otherwise check through ifconfig) with your credentials from NO-IP.

Update/create a file on your host machine `~\.ssh\config` with the following content:

```
Host pi
Hostname your-no-ip-host.org
	User pi
	LocalForward 9091 127.0.0.1:9091
	LocalForward 5050 127.0.0.1:5050
	LocalForward 32400 127.0.0.1:32400
```

This allows you to use a single command to log to your Raspberry:

```bash
$ ssh pi
```

Additionaly, it forwards some ports to your local machine for easier access. Ports 9091 (transmission), 5050 (flexget) and 32400 (plex) from Raspberry are now accesible from your local machine through 127.0.0.1:{port}.

You may see some log errors if the Raspberry isn't using those ports yet.

## Sudo (not sure if needed)

Add user to sudo group and don't require password for sudoers:

```bash
$ sudo usermod -a -G sudo pi
```

Edit `/etc/sudoers`:

```
%sudo   ALL=(ALL:ALL) NOPASSWD:ALL
```

## Docker

Install Docker:

```bash
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
```

Grant permission to user pi:

```bash
$ sudo usermod -a -G docker pi
```

Re login to enable docker permissions for user pi

(Optional) Uncomment last line in `/etc/default/docker` to save temporal files to an external drive.

```
export DOCKER_TMPDIR="/mnt/seagate/docker-tmp"
```

## docker-compose

Install docker-compose:

```bash
$ sudo apt update
$ sudo apt install -y python3-pip libffi-dev
$ sudo pip3 install docker-compose
```

## HDD

Mount HDD. Remember to add UUID to stab file for auto-mount on startup.

Look up HDD:

```bash
$ sudo su
$ fdisk -l
```

Format HDD:

```bash
$ fdisk /dev/sda
# remove partitions (d), create a new one (n) and save changes (w)
$ mkfs.ext4 /dev/sda
```

Look up UUID of HDD:

```bash
$ ls -l /dev/disk/by-uuid/
```

Edit `/etc/fstab` and add the following line to mount HDD on startup:

```
UUID=your-disk-uuid       /mnt/seagate    ext4    defaults   0       0
```

Create the folder for mount (`/mnt/seagate` in this case).

Run `mount -a` to mount HDD without restarting.

Give permissions to user pi:

```bash
$ chown -R pi.pi /mnt/seagate
```