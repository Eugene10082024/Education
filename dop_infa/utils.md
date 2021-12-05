## Дополнительные утилиты для работы с Linux

### Блочные устройства и работа с ними.

Блочное устройство (block device) — вид файла устройств в UNIX/Linux-системах, обеспечивающий интерфейс к устройству, реальному или виртуальному, в виде файла в файловой системе. С блочным устройством обеспечивается обмен данными блоками данных. Как правило, это устройства произвольного доступа, то есть можно указать, из какого именно места должен быть прочитан или записан блок данных. Данные при чтении или записи на блочное устройство буферизуются.Типичные примеры блочных устройств: жесткие диски, CD-ROM, flesh-накопитили

#### Информация о блочных устройствах хранится в каталоге /dev

        root@astra-ansible:/etc/ansible# ls -al /dev/sd*
        brw-rw---- 1 root disk 8, 0 дек  5 10:25 /dev/sda
        brw-rw---- 1 root disk 8, 1 дек  5 10:25 /dev/sda1
        brw-rw---- 1 root disk 8, 2 дек  5 10:25 /dev/sda2
        brw-rw---- 1 root disk 8, 5 дек  5 10:25 /dev/sda5

#### Вывод списка подключенных в данный момент блочных устройств:

        root@astra-ansible:/etc/ansible# lsblk
        NAME                           MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
        sda                              8:0    0   20G  0 disk 
        ├─sda1                           8:1    0  731M  0 part /boot
        ├─sda2                           8:2    0    1K  0 part 
        └─sda5                           8:5    0 19,3G  0 part 
          ├─astra--orel--01--vg-root   253:0    0 15,3G  0 lvm  /
          └─astra--orel--01--vg-swap_1 253:1    0    4G  0 lvm  [SWAP]
        sr0                             11:0    1 1024M  0 rom  
        root@astra-ansible:/etc/ansible# 

#### Вывод метаданных блочных устройств.

        root@astra-ansible:/etc/ansible# blkid
        /dev/sda1: UUID="056b1473-feca-487e-9f3d-faf267c28520" TYPE="ext2" PARTUUID="abcac8d5-01"
        /dev/sda5: UUID="HIZnJQ-jmSg-Hm6W-QJVt-yAa6-flWD-v9N3gp" TYPE="LVM2_member" PARTUUID="abcac8d5-05"
        /dev/mapper/astra--orel--01--vg-root: UUID="b041799f-b8e3-409c-be5b-732685489e78" TYPE="ext4"
        /dev/mapper/astra--orel--01--vg-swap_1: UUID="e14fdd96-39cf-4339-bada-e36660e34546" TYPE="swap"

#### Вывод сведений о разделах доступных носителей:    

          root@astra-ansible:/etc/ansible# partprobe -ds 
          /dev/sda: msdos partitions 1 2 <5>
          /dev/mapper/astra--orel--01--vg-swap_1: loop partitions 1
          /dev/mapper/astra--orel--01--vg-root: loop partitions 1
          root@astra-ansible:/etc/ansible# 

### Утилиты работы с блочными уcтройствами.

#### Утилита parted
          root@astra-ansible:/etc/ansible# parted
            (parted) print list
            Model: ATA QEMU HARDDISK (scsi)
            Disk /dev/sda: 21,5GB
            Sector size (logical/physical): 512B/512B
            Partition Table: msdos
            Disk Flags: 

            Number  Start   End     Size    Type      File system  Flags
             1      1049kB  768MB   767MB   primary   ext2         boot
             2      769MB   21,5GB  20,7GB  extended
             5      769MB   21,5GB  20,7GB  logical                lvm


            Model: Linux device-mapper (linear) (dm)
            Disk /dev/mapper/astra--orel--01--vg-swap_1: 4295MB
            Sector size (logical/physical): 512B/512B
            Partition Table: loop
            Disk Flags: 

            Number  Start  End     Size    File system     Flags
             1      0,00B  4295MB  4295MB  linux-swap(v1)


            Model: Linux device-mapper (linear) (dm)
            Disk /dev/mapper/astra--orel--01--vg-root: 16,4GB
            Sector size (logical/physical): 512B/512B
            Partition Table: loop
            Disk Flags: 

            Number  Start  End     Size    File system  Flags
             1      0,00B  16,4GB  16,4GB  ext4

#### Последовательность подготовки блочного устройства к подключению (утилиты parted, fsdisk, gparted).

1. Определям имя блочного устройства которое хотим подключить в помошью утилиты lsblk.

                root@astra-ansible:~# lsblk
                NAME                           MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
                sda                              8:0    0   20G  0 disk 
                ├─sda1                           8:1    0  731M  0 part /boot
                ├─sda2                           8:2    0    1K  0 part 
                └─sda5                           8:5    0 19,3G  0 part 
                  ├─astra--orel--01--vg-root   253:0    0 15,3G  0 lvm  /
                  └─astra--orel--01--vg-swap_1 253:1    0    4G  0 lvm  [SWAP]
                sdb                              8:16   0   20G  0 disk 
                sr0                             11:0    1 1024M  0 rom  
                
 У нас есть новое блочное устройство sdb. Будем его подключать к ОС Linux.
 
 2. Подключаемся к устройству утилитой parted.

                root@astra-ansible:~# parted /dev/sdb
                GNU Parted 3.2
                Using /dev/sdb
                Welcome to GNU Parted! Type 'help' to view a list of commands.
                (parted)       

3. Смотрим параметры устройства

                (parted) p free
                Error: /dev/sdb: unrecognised disk label
                Model: ATA QEMU HARDDISK (scsi)                                           
                Disk /dev/sdb: 21,5GB
                Sector size (logical/physical): 512B/512B
                Partition Table: unknown
                Disk Flags: 
                (parted)                        

4. Создаем таблицу разделов gpt и раздел

Почему gpt?

MBR – традиционная система дискового разделения, которая используется уже более 30 лет. Из-за её возраста в ней есть ряд серьёзных ограничений. Например, её нельзя использовать для разделения дисков, чей объём превышает 2Тб. Также MBR позволяет создать максимум четыре первичных раздела; при этом четвертый раздел, как правило, является «расширенным разделом», в котором можно создавать «логические разделы» (то есть, таким образом можно разделить последний раздел, чтобы добавить дополнительные разделы).

GPT – более современная модель разделения диска, которая пытается решить некоторые из проблем MBR. Системы, использующие GPT, могут делить диск на большее количество разделов (как правило, ограничения в этом смысле накладываются самой операционной системой). Кроме того, GPT не ограничивает размеры диска, а таблица разделов доступна в нескольких точках системы из соображений безопасности.

                (parted) mktable gpt
                (parted) mkpart                                                           
                Partition name?  []?                                                      
                File system type?  [ext2]? ext4                                           
                Start? 0%                                                                 
                End? 100%                                                                 
                (parted)                            

5. смотрим что получилось и выходим из parted

                (parted) p free
                Model: ATA QEMU HARDDISK (scsi)
                Disk /dev/sdb: 21,5GB
                Sector size (logical/physical): 512B/512B
                Partition Table: gpt
                Disk Flags: 

                Number  Start   End     Size    File system  Name  Flags
                        17,4kB  1049kB  1031kB  Free Space
                 1      1049kB  21,5GB  21,5GB  ext4
                        21,5GB  21,5GB  1032kB  Free Space

                (parted)                                                     

6. Форматирует созданный раздел /dev/sdb1 ext4

                root@astra-ansible:~# mkfs.ext4 /dev/sdb1
                mke2fs 1.43.4 (31-Jan-2017)
                Discarding device blocks: done                            
                Creating filesystem with 5242368 4k blocks and 1310720 inodes
                Filesystem UUID: ecc29878-41f8-4bc6-9692-59e7b07fe968
                Superblock backups stored on blocks: 
                        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
                        4096000

                Allocating group tables: done                            
                Writing inode tables: done                            
                Creating journal (32768 blocks): done
                Writing superblocks and filesystem accounting information: done   

                root@astra-ansible:~# 

7. Созданный раздел готов к использованию. Для его использования необходимо выполнить монтирование.

#### Монтирование блочных устройств.

Монтирование –  это процесс присоединения отформатированного раздела или диска к каталогу в файловой системе Linux. Доступ к содержимому накопителя можно получить с помощью этого каталога.

Устройства обычно монтируются в специальные выделенные пустые каталоги; если устройство было смонтировано в непустой каталог, то текущее содержимое этого каталога будет недоступно, пока устройство не демонтируют. Существует множество различных опций для монтирования, которые можно настроить, чтобы изменить поведение смонтированного устройства (например, открыть устройство только для чтения, чтобы защитить его данные от изменений).

Стандарт иерархии файловой системы рекомендует использовать для временно смонтированных файловых систем каталог /mnt или его подкаталоги. Однако этот стандарт не дает никаких рекомендаций относительно того, где хранить более постоянные устройства, потому вы можете выбрать для них любое удобное место. Обычно для этого также  используется каталог /mnt или его подкаталоги.

Монтирование выполняется с помощью утилиты mount.

Монтируем устройство /dev/sdb1 и каталогу /mnt

                root@astra-ansible:~# mount /dev/sdb1 /mnt
                root@astra-ansible:~# mount
                sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
                proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
                udev on /dev type devtmpfs (rw,nosuid,relatime,size=1979184k,nr_inodes=494796,mode=755,inode64)
                devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
                tmpfs on /run type tmpfs (rw,nosuid,noexec,relatime,size=402820k,mode=755,inode64)
                /dev/mapper/astra--orel--01--vg-root on / type ext4 (rw,relatime,errors=remount-ro)
                securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
                tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev,inode64)
                tmpfs on /run/lock type tmpfs (rw,nosuid,nodev,noexec,relatime,size=5120k,inode64)
                tmpfs on /sys/fs/cgroup type tmpfs (ro,nosuid,nodev,noexec,mode=755,inode64)
                systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=37,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=16911)
                mqueue on /dev/mqueue type mqueue (rw,relatime)
                hugetlbfs on /dev/hugepages type hugetlbfs (rw,relatime,pagesize=2M)
                debugfs on /sys/kernel/debug type debugfs (rw,relatime)
                configfs on /sys/kernel/config type configfs (rw,relatime)
                fusectl on /sys/fs/fuse/connections type fusectl (rw,relatime)
                /dev/sda1 on /boot type ext2 (rw,relatime)
                tmpfs on /run/user/999 type tmpfs (rw,nosuid,nodev,relatime,size=402816k,mode=700,uid=999,gid=999,inode64)
                tmpfs on /run/user/1001 type tmpfs (rw,nosuid,nodev,relatime,size=402816k,mode=700,uid=1001,gid=1002,inode64)
                /dev/sdb1 on /mnt type ext4 (rw,relatime)

Отмонтировать устройство можно с помощью команды umount

                umount /dev/sdb1
                или
                umount /mnt
                
После перезагрузки системы устройство не будет примонтировано.


#### 














 
 
                
                
                





