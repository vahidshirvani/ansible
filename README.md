# ansible
Setup remote server with ansible for running containerized docker apps. 
Running services are:
* wireguard
* qbittorrent
* rar2fs
* plex

## prerequisite
Make sure you can `ssh` into the remote machine. 
Preferably passwordless.
```bash
ssh ubuntu@192.168.1.149
ssh-copy-id ubuntu@192.168.1.149
```
Make sure you have ansible installed and that remote machine can be pinged.
```bash
ansible --version
ansible-inventory -i inventory.yml --list
ansible all -i inventory.yml -m ping
```

## run
Run through the playbook with command
```bash
ansible-playbook -i inventory.yml playbook.yml
```

## speedup
Run these manually beforehand so that ansible can execute faster
```bash
sudo apt -y update
sudo apt -y dist-upgrade
sudo apt -y upgrade
docker pull linuxserver/qbittorrent
docker pull ghcr.io/wg-easy/wg-easy
docker pull linuxserver/plex
docker pull zimme/rar2fs
```

## cleanup
Free up some disk space sometimes by
```bash
sudo apt -y autoremove
sudo apt -y autoclean
docker system prune --all --force
```

## troubleshooting
Make sure port forwarding is configured in NAT router.
I prefer upper ranges such as 50000-60000 for obfuscation reasons.

| Name        | Direction  | Dst. IP       | Protocol | Public Port | Private Port |
|-------------|------------|---------------|----------|-------------|--------------|
| qbittorrent | wan to lan | 192.168.1.249 | tcp udp  | 56881       | 56881        |
| wireguard   | wan to lan | 192.168.1.249 | udp      | 51820       | 51820        |
| plex        | wan to lan | 192.168.1.249 | tcp      | 52400       | 32400        |
