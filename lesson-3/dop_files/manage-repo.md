### Полезные команды при работе с пакетным менеджером yum, которые могут пригодиться:

Установить пакет:

  yum install figlet 

Удалить пакет:

  yum remove figlet 

Переустановить пакет:

  yum reinstall figlet 

Найти пакет в репозиториях:

  yum search figlet 

Отобразить информацию о пакете:

  yum info figlet 

Обновить установленные пакеты:

  yum update 

Обновить конкретный установленный пакет:

  yum update figlet 

Обновить пакет до определенной версии:

  yum update-to

Показать историю:

yum history 

Вывести список включенных репозиториев:

 yum repolist 

Найти пакет, который предоставляет файл (например, /usr/bin/figlet):

 yum provides "*bin/figlet" 

Очистить кэш:

 yum clean all
