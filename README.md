# Домашнее задание к занятию "3.4. Операционные системы, лекция 2"

 1. Node_exporter
 
     Скачиваем нужный архив на свою машину [node_exporter](https://github.com/prometheus/node_exporter/releases) 
     
     Распаковываем `tar xvfz node_exporter-1.3.1.linux-386.tar.gz`
     
     Для удобства скопирую исполняемый файл в /usr/local/bin `sudo cp ~/Загрузки/node_exporter-1.3.1.linux-386/node_exporter /usr/local/bin` 
     
     Создам юнит файл systemd для его запуска `sudo systemctl edit --full --force node_exporter.service`
     ```
      [Unit]
      Description=Node Exporter
      Wants=network-online.target
      After=network-online.target
      [Service]
      User=node_exporter
      Group=node_exporter
      Type=simple
      ExecStart=/usr/local/bin/node_exporter
      [Install]
      WantedBy=multi-user.target
      ```
      
      Запускаем `sudo systemctl start node_exporter`
     
     Проверяем статус нашего сервиса `sudo systemctl status node_exporter`
     ![alt text](https://i.imgur.com/dOJLBFw.png)
     
     Подключаем для нашего сервиса автозапуск `sudo systemctl enable node_exporter`
     
     Дополнение: Необходимо проверить заранее не занят ли порт 9100, так как его использует node_exporter
     
     Скриншот работы Node Exporter с браузерной странички на нашем адрессе 
     ![alt text](https://i.imgur.com/uBGlaEp.png)
    
     
    
