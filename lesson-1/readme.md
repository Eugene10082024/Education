### Урок 1 Установка и настройка Linux
#### Расссматриваемые вопросы.

    1. Установка и настройка Oracle Virtual Box 
    2. Установка и настройка Astra Linux CE из iso образа в  ВМ под управлением Oracle Virtual Box
    3. Установка и настройка Vagrant 
    4. Работа Vagrant + Oracle Virtual Box для быстрого создания стендов c ВМ

### Установка и настройка Oracle Virtual Box
Шаг 1.

![picture](pic/oracle_vb_01.png)

Шаг 2.

![picture](pic/oracle_vb_02.png)

Шаг 3.

![picture](pic/oracle_vb_03.png)

Шаг 4.

![picture](pic/oracle_vb_04.png)

Шаг 5.

![picture](pic/oracle_vb_05.png)

Шаг 6.

![picture](pic/oracle_vb_06.png)

При первом входе желательно задать местонахождение файлов ВМ, чтобы потом с ними было проще работать.

![picture](pic/oracle_vb_07.png)

Для выволняем в основном меню: Файл -> настройки. После чего откровется окно в котором заем местонахождение файлов ВМ:  

![picture](pic/oracle_vb_08.png)

### Установка и настройка Linux из iso образа в  ВМ под управлением Oracle Virtual Box

Скачать iso Astra Linux CE можно c - https://download.astralinux.ru/astra/stable/orel/iso/

#### Создание пустой ВМ для установки Astra Linux CE.

![picture](pic/vm_cr_01.png)

![picture](pic/vm_cr_02.png)

![picture](pic/vm_cr_03.png)

![picture](pic/vm_cr_04.png)

![picture](pic/vm_cr_05.png)

![picture](pic/vm_cr_06.png)

![picture](pic/vm_cr_07.png)

![picture](pic/vm_cr_08.png)

![picture](pic/vm_cr_09.png)

#### Установка Astra Linux 

![picture](pic/vm_inst_01.png)

![picture](pic/vm_inst_02.png)

![picture](pic/vm_inst_03.png)

![picture](pic/vm_inst_04.png)

![picture](pic/vm_inst_06.png)

Установка описана в документе - https://astralinux.ru/products/astra-linux-common-edition/documents-astra-ce/instrukcziya-po-ustanovke-os-astra-linux-common-edition.pdf стр 13.

### Установка и настройка Vagrant 
Шаг 1.

![picture](pic/vagrant_01.png)

Шаг 2.

![picture](pic/vagrant_02.png)

Шаг 3.

![picture](pic/vagrant_03.png)

Шаг 4.

![picture](pic/vagrant_04.png)

Шаг 5.

![picture](pic/vagrant_05.png)

После установки vagrant и перезагрузки ПК выполняем проверку установленной версии .

        vagrant version

![picture](pic/vagrant_w_02.png)

Выводим функциональные возможности vagrant по созданию и управлению ВM
    
    vagrant --help
    
![picture](pic/vagrant_w_01.png)

Для нормальной работы дополнительного функционала используемого в конфигурационных файлах Vagrantfile необходимо установить plugin vagrant-vbguest

Для этого выполняем команду.
        
        vagrant plugin install vagrant-vbguest

![picture](pic/vagrant_w_03.png)

### Работа Vagrant + Oracle Virtual Box для быстрого создания стендов c ВМ.

В основы работы Vagrant погружаться не будем. О этом можно прочитать на сайте https://www.vagrantup.com/

Для проверения первого этапа обучения подготовлены 2 Vagrantfile c Astra Linux CE.

1. Vagrantfile_gui - файл позволяющий развернуть ВМ c графическим интерфейсом

https://github.com/Aleksey-10081967/Education/blob/main/lesson-1/vagrant-files/Vagrantfile_gui

2. Vagrantfile_gui - файл позволяющий развернуть ВМ c графическим интерфейсом

https://github.com/Aleksey-10081967/Education/blob/main/lesson-1/vagrant-files/Vagrantfile_nogui

Порядок действий следующий.

Шаг 1.

На ПК создается каталог в котором будет лежать Vagrantfile + создаться служебный каталог .vagrant для работы ВМ развернытых с помощью Vagrant.
(Например С:\vagrant\Astra_GUI\)

Шаг 2. 
В созданный каталог копируем файл Vagrantfile_gui и желательного переименовываем его в Vagrantfile

Шаг 3.
Запускаем cmd и переходим в каталог СС:\vagrant\Astra_GUI\

    cd С:\vagrant\Astra_GUI\
    
Шаг 4.
Выполняем команду создания ВМ с помощью vagrant
    
    vagrant up

Основные параменты создаваемой ВМ:

        Имя - astrace01

        IP - 192.168.56.2

        box - "rta/orelgui"

После первого запуска в начале vagrant загрует box file c ОС Asta c сайта - https://app.vagrantup.com/boxes/search

Это может занять значительное время. Проверить выполнение загрузки можно через Диспетчер задач - Производительность

![picture](pic/vagrant_w_04.png)

Ниже показан скрин-шот завершения создания ВМ. Если появились ошибки надо разбираться.

![picture](pic/vagrant_w_05.png)

После успешного создания ВМ можно к ней подключиться по ssh.
    
    vagrant ssh
    
Подключение выполняется под пользователем:
vagrant / vagrant

![picture](pic/vagrant_w_06.png)

После первого подключения по ssh к ВМ настоятельно рекомендую сделать reboot BM.
    
    sudo init 6

После перезагрузки ВМ можно подключиться через GUI

Для этого через иконку на рабочем столе 

![picture](pic/vagrant_w_08.png)

Открыть интерфейс Oracle Virtual Box

![picture](pic/vagrant_w_09.png)

B двойным щелчком мыши открыть графический вход в ВМ.

Пользователь входа: vagrant password:vagrant

![picture](pic/vagrant_w_10.png)

При развертывании ВМ дополнительно создается пользователь: 
superuser / Superuser54321

Дополнительно при развертывании ВМ устанавливается сервис xpd который позволяет с Windows подключаться к ВМ по RDP

![picture](pic/vagrant_rdp_01.png)

![picture](pic/vagrant_rdp_02.png)

![picture](pic/vagrant_rdp_03.png)







