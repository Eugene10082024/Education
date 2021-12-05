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
          GNU Parted 3.2
          Using /dev/sda
          Welcome to GNU Parted! Type 'help' to view a list of commands.
          (parted) help                                                             
            align-check TYPE N                        check partition N for TYPE(min|opt) alignment
            help [COMMAND]                           print general help, or help on COMMAND
            mklabel,mktable LABEL-TYPE               create a new disklabel (partition table)
            mkpart PART-TYPE [FS-TYPE] START END     make a partition
            name NUMBER NAME                         name partition NUMBER as NAME
            print [devices|free|list,all|NUMBER]     display the partition table, available devices, free space, all found partitions, or a particular partition
            quit                                     exit program
            rescue START END                         rescue a lost partition near START and END
            resizepart NUMBER END                    resize partition NUMBER
            rm NUMBER                                delete partition NUMBER
            select DEVICE                            choose the device to edit
            disk_set FLAG STATE                      change the FLAG on selected device
            disk_toggle [FLAG]                       toggle the state of FLAG on selected device
            set NUMBER FLAG STATE                    change the FLAG on partition NUMBER
            toggle [NUMBER [FLAG]]                   toggle the state of FLAG on partition NUMBER
            unit UNIT                                set the default unit to UNIT
            version                                  display the version number and copyright information of GNU Parted

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



