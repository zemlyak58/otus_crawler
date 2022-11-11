python -m venv crawler
source crawler/bin/activate

git clone git@github.com:express42/search_engine_crawler.git
pip install -r requirements.txt
python -u crawler/crawler.py URL

EXCLUDE_URLS='.*github.com' python -u crawler/crawler.py https://vitkhab.github.io/search_engine_test_site/

<!-- Создание образа в yc crawler -1-> -->
$ packer build -var-file=variables.json ./crawler_image.json
<!-- проверка созданного образа -->
$ yc compute image list
<!-- Запуск instance с помощью terraform -->
$ terraform apply
<!-- Создание docker-machine -->

<!-- # Создание instance ycloud -->
<!-- yc compute instance create \ -->
<!--   --name crawler-ci \ -->
<!--   --zone ru-central1-a \ -->
<!--   --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 \ -->
<!--   --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1804-lts,size=15 \ -->
<!--   --ssh-key ~/.ssh/appuser.pub -->

# Запуск docker-machine
docker-machine create \
  --driver generic \
  --generic-ip-address=`yc compute instance list | grep crawler-ci | awk '{print $10}'` \
  --generic-ssh-user ubuntu \
  --generic-ssh-key ~/.ssh/appuser \
  docker-crawler
<!-- Подключение к Docker host'у -->
eval $(docker-machine env docker-crawler)

# Подключение к Docker host'у
eval $(docker-machine env docker-host)

# Запуск контейнера с используем none-драйвера
docker run -ti --rm --network none joffotron/docker-net-tools -c ifconfig

# Запуск контейнера в сетевом пространстве docker-host'a не забыть устрановить $ sudo apt install net-tools
docker run -ti --rm --network host joffotron/docker-net-tools -c ifconfig

# Запуск контейнера в сетевом пространстве docker-host'a nginx
docker run --network host -d nginx

# На docker-host машине $ docker-machine ssh docker-host
sudo ln -s /var/run/docker/netns /var/run/netns
sudo ip netns

# Создаём сеть bridge-сеть
docker network create reddit --driver bridge

# Запуск проекта reddit с использование bridge-сети
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=reddit --network-alias=post zemlyak/post:1.0 
docker run -d --network=reddit --network-alias=comment zemlyak/comment:1.0 
docker run -d --network=reddit -p 9292:9292 zemlyak/ui:1.0 

# Запуск проекта с 2м bridge сетями
# Создание docker-сети
docker network create back_net --subnet=10.0.2.0/24
docker network create front_net --subnet=10.0.1.0/24
# Запуск контейнеров
docker run -d --network=back_net --name mongo_db --network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=back_net --name post --network-alias=post zemlyak/post:1.0 
docker run -d --network=back_net --name comment --network-alias=comment zemlyak/comment:1.0 
docker run -d --network=front_net --name ui -p 9292:9292 zemlyak/ui:1.0 

docker network connect front_net post
docker network connect front_net comment


# Проверяем где docker-host-ip можно посмотреть $docker-mashine list
http://<docker-host-ip>:9292

# Изучение сетевого стека
# connect ssh for docker-machine
docker-machine ssh docker-host
# install bridge-utils
sudo apt-get update && sudo apt-get install bridge-utils

# Для запуска docker-compose
# перейти в каталог src/
cd src/
# Запуск контейнеров
docker-compose up -d

#Для изменения базового имя проекта используем ключ -p
  -p, --project-name string        Project name
docker-compuse -p hello up -d

