# Домашнее задание к занятию "3.4. Операционные системы, лекция 2"

1. На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:

    * поместите его в автозагрузку,
    * предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на `systemctl cat cron`),
    * удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.


     ## Ответ
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
    
2. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.
      
      ## Ответ
      Перешел, для ознакомления с `metrics`, на http://localhost:9100/metrics
      
      * CPU `node_cpu_seconds_total{cpu="1",mode="system"} 7.31` `node_cpu_seconds_total{cpu="1",mode="user"} 14.8` `node_cpu_seconds_total{cpu="1",mode="idle"} 287.38`
      `node_cpu_seconds_total{cpu="0",mode="system"} 8.5` `node_cpu_seconds_total{cpu="0",mode="user"} 16.11` `node_cpu_seconds_total{cpu="0",mode="idle"} 277.67`
      
      * MEMORY `node_memory_MemAvailable_bytes 2.712117248e+09` `node_memory_MemFree_bytes 1.546317824e+09` `node_memory_MemTotal_bytes 4.1227264e+09`
      * DISK `node_disk_io_time_seconds_total{device="sda"} 152.728` `node_disk_read_bytes_total{device="sda"} 1.280625664e+09` `node_disk_read_time_seconds_total{device="sda"} 480.72` `node_disk_write_time_seconds_total{device="sda"} 446.18600000000004` 


    
