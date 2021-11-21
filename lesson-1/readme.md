### Урок 1 Установка и настройка Linux
#### Расссматриваемые вопросы.

    1. Установка и настройка Oracle Virtual Box 
    2. Установка и настройка Astra Linux CE из iso образа в  ВМ под управлением Oracle Virtual Box
    3. Установка и настройка Vagrant 
    4. Работа Vagrant + Oracle Virtual Box для быстрого создания стендов c ВМ

##### Установка и настройка Oracle Virtual Box
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

##### Установка и настройка Linux из iso образа в  ВМ под управлением Oracle Virtual Box

Скачать iso Astra Linux CE можно c - https://download.astralinux.ru/astra/stable/orel/iso/

###### Дейстия проводимые на Oracle Virtual Box Для запуска установки.

###### Установка Astra Linux 

Установка описана в документе - https://astralinux.ru/products/astra-linux-common-edition/documents-astra-ce/instrukcziya-po-ustanovke-os-astra-linux-common-edition.pdf стр 13.



##### Установка и настройка Vagrant 
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

##### Работа Vagrant + Oracle Virtual Box для быстрого создания стендов c ВМ.

В основны работы Vagrant погружаться не будем. О этом можно прочитать на сайте 

https://www.vagrantup.com/

Для проверения первого этапа обучения подготовлены 2 Vagrantfile.


https://github.com/Aleksey-10081967/Education/blob/main/lesson-1/vagrant-files/Vagrantfile_gui

https://github.com/Aleksey-10081967/Education/blob/main/lesson-1/vagrant-files/Vagrantfile_nogui



![picture](pic/vagrant_w_04.png)

![picture](pic/vagrant_w_05.png)

![picture](pic/vagrant_w_06.png)

![picture](pic/vagrant_w_07.png)

![picture](pic/vagrant_w_08.png)

![picture](pic/vagrant_w_09.png)

![picture](pic/vagrant_w_10.png)










