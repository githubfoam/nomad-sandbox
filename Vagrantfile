# -*- mode: ruby -*-
# vi: set ft=ruby :
$script = <<SCRIPT
echo "==========================Install Docker================================="
export DEBIAN_FRONTEND=noninteractive
sudo apt-get update
sudo apt-get remove docker docker-engine docker.io
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common -y
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg |  sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
sudo apt-get update
sudo apt-get install -y docker-ce
# Restart docker to make sure we get the latest version of the daemon if there is an upgrade
sudo service docker restart
# Make sure we can actually use docker as the vagrant user
sudo usermod -aG docker vagrant
sudo docker --version
# Packages required for nomad & consul
sudo apt-get install unzip curl vim -y
NOMAD_VERSION=0.8.6
echo "==========================Install Nomad================================="
cd /tmp/
curl -sSL https://releases.hashicorp.com/nomad/${NOMAD_VERSION}/nomad_${NOMAD_VERSION}_linux_amd64.zip -o nomad.zip
unzip nomad.zip
sudo install nomad /usr/bin/nomad
sudo mkdir -p /etc/nomad.d
sudo chmod a+w /etc/nomad.d
echo "==========================Install Consul================================"
CONSUL_VERSION=1.4.0
curl -sSL https://releases.hashicorp.com/consul/${CONSUL_VERSION}/consul_${CONSUL_VERSION}_linux_amd64.zip > consul.zip
unzip /tmp/consul.zip
sudo install consul /usr/bin/consul
(
cat <<-EOF
  [Unit]
  Description=consul agent
  Requires=network-online.target
	After=network-online.target

  [Service]
  Restart=on-failure
  ExecStart=/usr/bin/consul agent -dev
  ExecReload=/bin/kill -HUP $MAINPID

  [Install]
  WantedBy=multi-user.target
EOF
) | sudo tee /etc/systemd/system/consul.service
sudo systemctl enable consul.service
sudo systemctl start consul
for bin in cfssl cfssl-certinfo cfssljson
do
  echo "==========================Installing $bin================================"
  curl -sSL https://pkg.cfssl.org/R1.2/${bin}_linux-amd64 > /tmp/${bin}
  sudo install /tmp/${bin} /usr/local/bin/${bin}
done
nomad -autocomplete-install
SCRIPT
Vagrant.configure(2) do |config|
  #config.vm.box_check_update = false
  config.vm.box = "bento/ubuntu-19.10"
  config.vm.hostname = "nomad-sandbox"
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.name = "nomad-sandbox"
    # vb.memory = "1024" #OK
    vb.memory = "4096"
    vb.cpus = 1
  end
  config.vm.provision "shell", inline: $script, privileged: false
  config.vm.provision "shell", inline: <<-SHELL
  echo "===================================================================================="
                            hostnamectl status
  echo "===================================================================================="
  echo "         \   ^__^                                                                  "
  echo "          \  (oo)\_______                                                          "
  echo "             (__)\       )\/\                                                      "
  echo "                 ||----w |                                                         "
  echo "                 ||     ||                                                         "
  SHELL
  # Expose the nomad api and ui to the host
  config.vm.network "forwarded_port", guest: 4646, host: 4646, auto_correct: true
end
