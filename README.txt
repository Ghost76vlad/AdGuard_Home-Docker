
Описание docker-compose

version: "3.9"
services:
  web-server:								//имя сервиса. может быть любое
    image: adguard/adguardhome				//имя образа
    container_name: adguardhome				//имя нашего контейнера
    networks:
      default:
        ipv4_address: 10.10.10.4			//назначаем ip контейнеру
    volumes:
    - /data/adguardhome/own/workdir:/opt/adguardhome/work
    - /data/adguardhome/own/confdir:/opt/adguardhome/conf
    ports:
      - 53:53/tcp
      - 53:53/udp
      - 67:67/udp
      - 68:68/udp
      - 80:80/tcp
      - 443:443/tcp
      - 443:443/udp
      - 784:784/udp
      - 853:853/tcp
      - 853:853/udp
      - 3000:3000/tcp
      - 5443:5443/tcp
      - 5443:5443/udp
      - 8853:8853/udp
    restart: always							//говорим, чтобы контейнер запускался после рестарта сервера.
networks:
 default:									//создаем сеть
    name: MyNet01	
    driver: macvlan							//указываем режим сети
    driver_opts:							//привязываем подсеть контейнеров к сетевому интерфейсу хоста 
      parent: ens18
    ipam:
      config:
        - subnet: 10.10.10.0/24				//задаем подсеть для контейнеров
          ip_range: 10.10.10.170/32			//указываем пул адресов для выдачи контейнерам по DHCP
          gateway: 10.10.10.1
          