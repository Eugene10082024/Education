## Загрузка ОС, Процессы и прочее

### Загрузка ОС.
Загрузка ОС включает в себя следующие этапы.

1. Включение питания

       Загрузка BIOS/UEFI из NVRAM
       Тест “железа”
       Выбор загрузочного устройства (диск, сеть)

2. Запуск загрузчика

       GRUB
       Выбор в какое ядро грузиться

3. Загрузка ядра
      
       Создание структуры данных ядра
       Старт PID 1 (init/systemd) 

## Systemd

Systemd – это система инициализации и системный менеджер

![picture](dop_files/systemd_1.png)

### Инициализация процессов

Запускается с PID 1

запускает остальные части системы

параллельно!

В systemd целью большинства действий являются юниты – ресурсы, которыми systemd может управлять. Юниты делятся на категории по типам ресурсов, которые они представляют. Юниты определяются в так называемых юнит-файлах. Тип каждого юнита можно определить по суффиксу в конце файла.

Для задач управления сервисами предназначены юнит-файлы с суффиксом .service. Однако в большинстве случаев суффикс .service можно опустить, так как система systemd достаточно умна, чтобы без суффикса определить, что нужно делать при использовании команд управления сервисом.

#### Основные команды управления юнитами на примере - сервисов)

Запуск сервиса, где application имя сервиса. (например - nginx)

      sudo systemctl start nginx.service
       
Остановка сервиса.

      sudo systemctl stop nginx.service
       
Перезапуск сервиса       

      sudo systemctl restart  nginx.service
       
Перезагрузка конфигурационных файлов сервиса

      sudo systemctl reload nginx.service
              
Добавление сервиса в автозагрузку

      sudo systemctl enable nginx.service
       
Удаление сервиса из автозагрузки

       sudo systemctl disable nginx.service

Проверка статуса сервиса

       sudo systemctl status
       
Запрос списка текущих юнитов systemd       

       sudo systemctl list-units

фильтр –type=. Он позволяет отфильтровать юниты по типу

       systemctl list-units --type=service
       
Systemd может блокировать юнит  (автоматически или вручную), создавая симлинк на /dev/null. Это называется маскировкой юнитов и выполняется командой mask.

       sudo systemctl mask nginx.service
       
Теперь сервис Nginx не будет запускаться автоматически или вручную до тех пор, пока включена маскировка.

### Анализ запуска процессов systemd

#### root@pc-asarafanov-01:~# systemd-analyze 

              Startup finished in 4.248s (kernel) + 4.339s (userspace) = 8.588s

#### root@pc-01:~# systemd-analyze critical-chain

The time after the unit is active or started is printed after the "@" character.
The time the unit takes to start is printed after the "+" character.

              graphical.target @4.105s
              └─multi-user.target @4.105s
                └─vboxweb-service.service @4.100s +5ms
                  └─network-online.target @4.099s
                    └─NetworkManager-wait-online.service @889ms +3.209s
                      └─NetworkManager.service @838ms +50ms
                        └─network-pre.target @837ms
                          └─firewalld.service @549ms +288ms
                            └─polkit.service @1.206s +16ms
                              └─basic.target @517ms
                                └─sockets.target @517ms
                                  └─acpid.socket @517ms
                                    └─sysinit.target @515ms
                                      └─systemd-update-utmp.service @512ms +2ms
                                        └─systemd-tmpfiles-setup.service @502ms +9ms
                                          └─local-fs.target @501ms
                                            └─run-user-999.mount @960ms
                                              └─local-fs-pre.target @198ms
                                                └─lvm2-monitor.service @138ms +60ms
                                                  └─lvm2-lvmetad.service @153ms
                                                    └─system.slice @135ms
                                                      └─-.slice @125ms
 #### root@pc-01:~# systemd-analyze blame
 
          3.209s NetworkManager-wait-online.service
           381ms vboxdrv.service
           288ms firewalld.service
           235ms libvirtd.service
           173ms dev-mapper-sarafanov\x2d\x2dgr05\x2dsarafanov\x2d\x2dusr.device
           172ms dev-mapper-sarafanov\x2d\x2dgr01\x2dsarafanov\x2d\x2droot.device
           144ms systemd-fsck@dev-disk-by\x2duuid-2e52f139\x2d20ee\x2d4671\x2d88ee\x2dc808a5f27133.service
           143ms apt-daily.service
           129ms gpm.service
           129ms astra-orientation.service
           126ms systemd-fsck@dev-disk-by\x2duuid-bafe308c\x2dffee\x2d420e\x2da8dd\x2d7ed1eb874f4f.service
            95ms apt-daily-upgrade.service
            87ms mnt-disk\x2dvm.mount
            73ms systemd-sysctl.service
            62ms mnt-disk\x2ddata.mount
            60ms lvm2-monitor.service
            58ms dev-mapper-sarafanov\x2d\x2dgr03\x2dsarafanov\x2d\x2dswap.swap
            57ms keyboard-setup.service
            50ms NetworkManager.service
            48ms quota.service
            46ms ssh.service
            44ms networking.service
            41ms systemd-udev-trigger.service
            39ms dnsmasq.service
            39ms rpcbind.service
            35ms systemd-logind.service
            34ms systemd-udevd.service
            33ms rsyslog.service
            32ms udisks2.service
            30ms systemd-journald.service
            30ms user@999.service
            29ms avahi-daemon.service
            29ms fly-dm.service
            29ms systemd-modules-load.service
            28ms upower.service
            22ms systemd-fsck@dev-disk-by\x2duuid-de1cf4f3\x2d6b96\x2d4d97\x2d997c\x2dfaacf9d83e29.service
            21ms plymouth-quit.service
            21ms plymouth-quit-wait.service
            20ms boot.mount
            20ms systemd-fsck@dev-mapper-sarafanov\x2d\x2dgr07\x2dsarafanov\x2d\x2dtmp.service
            18ms quotarpc.service
            18ms home.mount
            17ms systemd-tmpfiles-setup-dev.service
            16ms systemd-journal-flush.service
            16ms polkit.service
            14ms user@1001.service
            14ms plymouth-start.service
            14ms systemd-fsck@dev-mapper-sarafanov\x2d\x2dgr06\x2dsarafanov\x2d\x2dopt.service
            13ms systemd-fsck@dev-mapper-sarafanov\x2d\x2dgr04\x2dsarafanov\x2d\x2dvar.service
            13ms tmp.mount
            13ms opt.mount
            13ms var.mount
            11ms xrdp.service
            11ms plymouth-read-write.service
            10ms systemd-fsck@dev-mapper-sarafanov\x2d\x2dgr02\x2dsarafanov\x2d\x2dhome.service

