## HOW TO DEPLOY THE HUMAN-BESTS-FRIENDS APP

# Clone the repository
git clone https://github.com/enzok00/esiea-ressources.git

# Go to esiea-ressources directory
cd esiea-ressources

# Install docker, configure and reboot
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG docker $USER
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
sudo reboot

# Create networks
docker network create cats-or-dogs-network
docker network create back-tier
docker network create front-tier

# Build the images
docker-compose -f docker-compose.build.yml build

# Launch the app
docker-compose up -d
