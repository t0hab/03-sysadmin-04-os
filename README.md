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
     ![alt text](https://i.ibb.co/LNmTr8D/image.png)
     
     Подключаем для нашего сервиса автозапуск `sudo systemctl enable node_exporter`
     
     Дополнение: Необходимо проверить заранее не занят ли порт 9100, так как его использует node_exporter
     
     Скриншот работы Node Exporter с браузерной странички на нашем адрессе 
     ![alt text](https://i.ibb.co/4sD4trk/image.png)
    
2. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.
      
      ## Ответ
      Перешел, для ознакомления с `metrics`, на http://localhost:9100/metrics
      
      * CPU `node_cpu_seconds_total{cpu="1",mode="system"} 7.31` `node_cpu_seconds_total{cpu="1",mode="user"} 14.8` `node_cpu_seconds_total{cpu="1",mode="idle"} 287.38`
      `node_cpu_seconds_total{cpu="0",mode="system"} 8.5` `node_cpu_seconds_total{cpu="0",mode="user"} 16.11` `node_cpu_seconds_total{cpu="0",mode="idle"} 277.67`
      
      * MEMORY `node_memory_MemAvailable_bytes 2.712117248e+09` `node_memory_MemFree_bytes 1.546317824e+09` `node_memory_MemTotal_bytes 4.1227264e+09`
      * DISK `node_disk_io_time_seconds_total{device="sda"} 152.728` `node_disk_read_bytes_total{device="sda"} 1.280625664e+09` `node_disk_read_time_seconds_total{device="sda"} 480.72` `node_disk_write_time_seconds_total{device="sda"} 446.18600000000004` 
      * NETWORK `node_network_receive_errs_total{device="enp0s3"} 0``node_network_receive_bytes_total{device="enp0s3"} 1.486831e+06` `node_network_transmit_bytes_total{device="enp0s3"} 130269` `node_network_transmit_errs_total{device="enp0s3"} 0`

3. Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata). После успешной установки:
   * в конфигурационном файле `/etc/netdata/netdata.conf` в секции [web] замените значение с localhost на `bind to = 0.0.0.0`,
    * добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте `vagrant reload`:

    ```bash
    config.vm.network "forwarded_port", guest: 19999, host: 19999
    ```

    После успешной перезагрузки в браузере *на своем ПК* (не в виртуальной машине) вы должны суметь зайти на `localhost:19999`. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.
    
     ## Ответ
     
     ![node_exporter](https://i.ibb.co/59Hgk17/image.png)

4. Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?

     ## Ответ
      
     Да. Можно загрепать для удобного визуала
     ![node_exporter](https://i.ibb.co/PmHdQJb/image.png)


    
