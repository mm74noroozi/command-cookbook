# server setup
## docker-compose setup
 ```
 #Uninstall old versions
 sudo apt-get remove docker docker-engine docker.io containerd runc
 sudo apt-get update
 sudo apt-get install \
      ca-certificates \
      curl \
      gnupg \
      lsb-release

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
   "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io

systemctl enable docker
systemctl start docker

#docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```
## fail2ban
```
apt install fail2ban
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
nano /etc/fail2ban/jail.local

#bantime=2h
#maximumretry = 3
# [sshd] 
# enabled = true

systemctl enable fail2ban

systemctl start fail2ban

fail2ban-client status sshd
```
## nginx
```
apt install nginx -y
systemctl enable nginx
systemctl start nginx
```
## ssh config
```
cd /etc/ssh/
nano sshd_config
#permitrootlogin -> no
#ssh port 22 -> ?
# clientaliveinterval 100
systemctl restart sshd
```
## ufw 
```
apt install ufw -y
systemctl enable ufw
ufw enable
ufw allow 80,443,22/tcp
ufw status
```
## gitlab runner
```
wget https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64 -O  /usr/local/bin/gitlab-runner
chmod +x  /usr/local/bin/gitlab-runner
mkdir /home/gitlab-runner
gitlab-runner install --user=root --working-directory=/home/gitlab-runner
gitlab-runner start
```
## gitlab runner debug
```
gitlab-runner --debug run
```
## mc
```
wget https://dl.min.io/client/mc/release/linux-amd64/mc  -O  /usr/local/bin/mc
chmod +x  /usr/local/bin/mc
```
## Fish Activate
```bash
apt-add-repository ppa:fish-shell/release-3
apt install fish
chsh -s /usr/bin/fish
curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish
omf themes theme
omf install https://github.com/dracula/fish
```
## traefik
```bash
mkdir -p /etc/traefik
mkdir -p /etc/traefik/conf.d
cd /etc/traefik
wget https://github.com/traefik/traefik/releases/download/v2.9.6/traefik_v2.9.6_linux_amd64.tar.gz
tar -xzvf traefik_v2.9.6_linux_amd64.tar.gz
mv traefik /usr/local/bin/
chmod +x /usr/local/bin/traefik
wget https://repo.s3.ir-thr-at1.arvanstorage.ir/traefik.service -O /etc/systemd/system/traefik.service
wget https://repo.s3.ir-thr-at1.arvanstorage.ir/traefik.yml -O /etc/traefik/traefik.yml

wget https://repo.s3.ir-thr-at1.arvanstorage.ir/middleware.yml -O /etc/traefik/conf.d/middleware.yml

systemctl enable traefik
systemctl restart traefik
systemctl status traefik
```
## v2ray
install
```bash
curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh | bash
curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh | bash
wget https://tange.mn-service.ir/alborz/alborz.mnse.store -O /usr/local/etc/v2ray/alborz.mnse.store
systemctl restart v2ray@alborz.mnse.store
```
uninstall 
```
curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh | bash --remove
```
