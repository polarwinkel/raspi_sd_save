# raspi_sd_save
An Ansible-Role to protect your Raspberry-Pi-SD-Card from it's early death / Keep your Raspi from eating sd-cards

## Why this is here

My Raspberry-Pi's keep eating my micro-sd-cards like - well - like raspberrys.

This is, as I think, due to unfortunate default settings in the raspbian image.

An sd-card is not suitable to act as a swap or for permanent logging. So there is a need for raspbian to stop doing it.

## What this does

This Ansible-role will add these lines on your Raspi to the `/etc/fstab`-file

```
tmpfs  /tmp  tmpfs  defaults,noatime,nosuid,size=100m  0 0
tmpfs  /var/tmp  tmpfs  defaults,noatime,nosuid,size=30m  0 0
tmpfs  /var/log/nginx  tmpfs  defaults,noatime,nosuid,mode=0666,size=30m  0 0
tmpfs  /var/spool/mqueue  tmpfs  defaults,noatime,nosuid,mode=0700,gid=12,size=30m  0 0
```

to write the temporary files to a (limited) tmpfs, meaning it will not we written to the sd-card, but will only be kept in the RAM.

Just be aware that your logs will be lost on reboot by this!

Furthermore the swap will be permanently turned of by these commands:

```
sudo dphys-swapfile swapoff
sudo dphys-swapfile uninstall
sudo update-rc.d dphys-swapfile remove
```

(you can check the success with `free -m`)

## How to use this

If you are comfortable with Ansible you probably won't read this.

If you are not DONT clone this, just appy the changes described before manually.

Or have a loog at [Ansible](https://www.ansible.com/)!
