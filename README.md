# Продвинутое создание сервера
 `> Данная статья будет немного переписана и вам станет проще ее изучать :)`

![image](https://user-images.githubusercontent.com/77334306/146064955-f7458d87-38c0-40dd-8634-e27dc2a150b5.png)
![image](https://user-images.githubusercontent.com/77334306/146064668-abc41204-a41e-40f8-a098-6a5a992ad5be.png)
![image](https://user-images.githubusercontent.com/77334306/146064892-06b3d01d-02fe-48cb-8618-08536de0ad3f.png)
![image](https://user-images.githubusercontent.com/77334306/146582420-c57493a7-decf-49a8-b248-e29add8f8afe.png)
![image](https://user-images.githubusercontent.com/77334306/146582380-00aee364-fe49-4a47-bd7c-fdbf1a10412a.png)
![image](https://user-images.githubusercontent.com/77334306/146998663-f98e5ea0-cf90-4f39-a34a-1f20a2afe4d0.png)



### Создайте свой личный Minecraft Проект с использованием продвинутой и удобной информации`

```
Версия документа v2.0

* Приоритет обновления данного репозитория был повышен, вы можете получать дополнительную информацию *
* Намного чаще. Данная статья создана для удобства в управлении вашим Проектом в Minecraft + самой системой Linux *

* На системах Арч линукс нет команды apt. Учтите этот факт при настройке вашей UNIX подобной системы *

Некоторые рекомендации из пунктов могут работать некорректно на некоторых системах
За подробной поддержкой обращайтесь в мой дискорд - https://discord.gg/7XkGYJbtZg

Для полноценной настройки рекомендую использовать Adoptium OpenJDK LTS Java

<  !  >  ПОЖАЛУЙСТА ЧИТАЙТЕ ВНИМАТЕЛЬНО И НЕ ПРЕДЪЯВЛЯЙТЕ ПРЕТЕНЗИЙ АВТОРУ СТАТЬИ  <  !  >
```
# Основные ссылки на контент
[OpenJDK](https://adoptium.net/) __| Java |__

[FabricMC](https://fabricmc.net/) __| Fabric |__

[PaperMC](https://papermc.io/) __| Server Software |__

[PurPur](https://purpurmc.org/) __| Server Software |__

[Pufferfish](https://ci.pufferfish.host/) __| Server Software |__

[Velocity Website](https://velocitypowered.com/) [Velocity From PaperMC](https://papermc.io/downloads#Velocity) __| Proxy Software |__

# Настройка выделенных и виртуальных серверов
### Базовые компоненты, архивация файлов, настройка безопасности

- Базовые компоненты для вашего сервера
- Все команды выполняются от ~ root пользователя, либо через sudo

### Обновление пакетов машины
```
sudo apt update

sudo apt upgrade


# Установить сразу все с авто соглашением

sudo apt update -y && sudo apt upgrade -y
```
### Специально для Linux (CentOS 8)
```
yum

yum update

yum upgrade

dnf

sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

dnf install htop

dnf install screen
```

### Полезные утилиты для вашего сервера
```
sudo apt install htop  -  Утилита для мониторинга всех запущенных процессов (Подобно top, но красивее)

sudo apt install screen  -  Важная утилита для создания сессий на вашей серверной машине

sudo apt install zip unzip  -  Утилита для архивации файлов в .zip

sudo apt install iptables  -  Полезная утилита для настройки IPv4 и IPv6 флагов (Firewall) (Можно управлять портами)

apt install neofetch  -  Утилита для красивого отображения вашей ОС и некоторых параметров

apt install fontconfig - Данный пакет шрифтов может потребоваться для некоторых плагинов*


# Удобная установка всех полезных пакетов через 1 команду

sudo apt install htop screen zip unzip iptables nload neofetch fontconfig smartmontools firewalld -y
```
- Обычно предустановлена на Ubuntu, но в нашем случае Debian ОС. Выполняет команды от имени root

```
apt install sudo
```

- Если потребуется подтвердить установку, то подтвердите отправив консоли Y (y), (-y syntax)



### Может решить вашу проблему с портами на Oracle Cloud - откройте порт UDP/TCP 25565
```
sudo apt install firewalld

sudo firewall-cmd --permanent --zone=public --add-port=25565/tcp

sudo firewall-cmd --permanent --zone=public --add-port=25565/udp

sudo firewall-cmd --reload

# Важно! В конфиге server.properties нужно указывать IP: 0.0.0.0
# Если у вас Bungeecord / Velocity, укажите в конфиге Bungeecord / Velocity IP: 0.0.0.0:25565

# Обратите внимание, что firewalld по умолчанию закрывает все* порты
# Обратите внимание, что ufw по умолчанию закрывает все** порты ( < ! > Может быть опасно < ! > )

* - Под словом все имеется 25565 и т.д. Порт 22 к примеру НЕ БУДЕТ закрыт!
** - Данная встроенная FireWall утилита по умолчаю может блокировать даже SSH/SFTP порт, рекомендуем сразу открыть его
через команда $ sudo ufw allow 22/tcp
```
### Установка Java на вашу серверную машину
- Вы научитесь легко и просто устанавливать и удалять Java с вашего сервера
- Для начала зайдите в SFTP клиент и перейдите в раздел ~/opt (Можно любой, но этот в качестве основы)
- В Linux изначально придумана команда для упаковки архивов и распаковки архивов
- Использовать команду tar можно с использованием следующих параметров:
```
# -c | --create — создать архив
# -a | --auto-compress — дополнительно сжать архив с компрессором который автоматически определить по расширению архива.
# -r | --append — добавить файлы в конец существующего архива
# -x | --extract, --get — извлечь файлы из архива
# -f | --file — указать имя архива
# -t | --list — отобразить список файлов и папок в архиве
# -v | --verbose — выводить список обработанных файлов
# -u | --update — Обновить архив новыми файлами
# -d | --diff, --delete — Проверить начилие архивов, удалить файл из архива
```

- Пример перемещения файлов по системе Linux (Не выполняйте эти команды просто так!)
```
# < ! > НЕ ОБЯЗАТЕЛЬНЫЕ КОМАНДЫ < ! >

mv /home/others/Test /others2

# Также вы можете использовать флаг* -v чтобы увидеть подроную информацию о процессе
# ~ — тильда, дает понять системе, что это корневой каталог root (~)
# Т.е ~/others2 и т.д

mv -v ~/home/others/Test ~/others2



# Вы можете также использовать приставку sudo к команде mv

# Теперь вы знакомы с командой для перемещения файлов, но рекомендуется еще раз закрепить материал
# Попробуйте данные команды на каком-то пустом сервере, либо можете установить WSL2 на вашу систему
# Рекомендуемый дистрибутив ОС — Ubuntu,Debian
```

### Начало процесса установки Java на ваш виртуальный/выделенный сервер
- Установка и распаковка архива при помощи "tar" — встроенный архиватор в Linux
```
# Архив уже должен быть установлен / перемещен в выбранную вами директорию
# Чтобы установить архив, вы можете использовать в качестве передачи файлов SFTP приложение
# WinSCP, либо же скачать сразу из консоли - wget <url>
# Команду писать без <>

# Теперь давайте его распакуем — после распаковки появится папка с нашей Java
# Выполните команду:

tar xf ИМЯ.tar.gz

# Обычно все пакеты имеют расширение .tar.gz
# Но мы ведь как раз используем "tar" ;)

# Например архив с Java называется:
OpenJDK16U-jdk_x64_linux_hotspot_16.0.1_9.tar.gz

# < ! > Название может быть иное, пожалуйста узнайте это < ! >

# Для распаковки архива потребувется ввести всего одну команду:

tar xf OpenJDK16U-jdk_x64_linux_hotspot_16.0.1_9.tar.gz

# На момент создания статьи последняя Java, которую лично я проверял у себя на Проектах
# Выполните данные команды для скачивания архива:

cd ДИРЕКТОРИЯ

sudo wget ССЫЛКА

... Далее команды распаковки и все действия выше

# ССЫЛКУ брать отсюда - https://adoptium.net/releases.html

# Если все сделано верно, то вы успешно распаковали Java в директорию
```

### Создание "Линка" для нашего файла Java в папке с самой Java
- Вы можете подробно изучить про команду ln и ее параметры
```
# Линки — тоже самое что ярлык, те кто когда либо имели систему Windows / MacOS 
# Могут знать про создание ярлыка при помощи клика ПКМ по файлу
# Но у нас ведь нет интерфейса, поэтому будем использовать команду "ln"
# *Кстати можно делать линки также через WinSCP — Клиент SFTP, FTP . . .
# Создаем ярлык (линк) для нашего исполняемого файла
# Выполните команду:

ln -svf /opt/.../bin/java /usr/bin/java

# Заместо ... пишем название папки с Java — /opt/jdk-16.0.1+9/bin/java

ln -svf /opt/jdk-16.0.1+9/bin/java /usr/bin/java

# Если вы все сделали правильно, то вы успешно установили Java на вашу машину
```

### Удаление Java с нашего сервера
- Многие скажут, что есть команда sudo apt remove *java* и т.д, но это самый простой способ — можно также и через команды 
```
# Для начала перейдем в корень сервера ~
# Далее переходим по пути ~/usr/bin
# Используем удобный вам способ для поиска файлов — мне было удобно через WinSCP Клиент
# Находим файл "Java" — он должен быть единственный в данной директории!
# Спокойно без боязни удаляем его — Готово вы удалили активную Java с вашего сервера, однако
# Она все еще существует как папку и архив по пути ~/opt
# Можно сделать это через команды

cd usr

cd bin

sudo rm java

# Проверить наличие активной Java, можно введя команду

java -version

# При успехе, у вас должно написаться, что Java не найдена
```

### Настройка безопасного входа на сервер - Linux
- В качестве альтернативы простым паролям, мы будем использовать Rsa_Keys с форматом шифрования SHA

- Генерация и установка ключей на сервер
```
# Открываем приложение основной терминал системы (Terminal, Powershell, Konsole (Manjaro) . . .)

ssh-keygen

ssh-keygen -b 4096 # Генерация более надежного ключа

ssh-keygen -b 8192 # Генерация ключа с повышенным битным шифрованием

# Далее внимательно читаем логи, вы уже почти создали пару ключей на вашем ПК ~/users/ВашЮзер/.ssh

# Чтобы передать ключ на наш сервер, воспользуемся данной командой (Вам потребуется войти с использованием "старого" пароля)

cat ~/.ssh/id_rsa.pub | ssh USER@IP "mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && chmod -R go= ~/.ssh && cat >> ~/.ssh/authorized_keys"

# ... USER@IP ...
# Подставьте ваши данные заместо шаблона
# USER — Логин, ваш пользователь на серверной машине
# IP — Ваш IP от серверной машины


# ВНИМАНИЕ! Мы начинаем отключать парольную аутентификацию на сервере, будьте аккуратны
# *Автор не несет ответвенности, если вы вдруг сломаете вход на вашу серверную машину,
# Обязательно создавайте бэкапы ваших игровых серверов*


# Перед этим убедитесь, что у вас установлены пакеты:

apt install sudo

apt install nano

# sudo — Нужно использовать если у вас основной user != root
# nano — Удобный редактор текста через SSH

# Как сохранить файл через nano

# CTRL + X , Y (yes), ENTER

# Готово, теперь вы умеете сохранять файлы, но все таки перейдем к отключению парольной авторизации
# Вводим команду:

sudo nano /etc/ssh/sshd_config

# Вам нужно найти или написать данную строчку:

PasswordAuthentication no

# После сохраняем файл (Мы используем nano в качестве редактора текстовых файлов)
# Далее нам необходимо перезагрузить SSH клиент
# < ! > Советуем проверить доступ к SSH < ! >
# Вернитесь в PowerShell и введите

ssh USER@IP

ssh USER@IP -i ./ключ

# Если вы пытаетесь зайти через GNU Linux, то используйте команду ниже

sudo ssh USER@IP -i ключ

# ... USER@IP
# Подставьте ваши данные заместо шаблона
# USER — Логин, ваш пользователь на серверной машине
# IP — Ваш IP от серверной машины

# Далее если все хорошо, перезагружаем ssh

sudo systemctl restart ssh

# Готово, теперь зайти на вашу серверную машину через пароли не получится, используем только ключи авторизации (rsa)
```

### Различные команды и бенчи, которые возможно вам понадобятся
- В основном подойдет для мониторинга и наблюдения за жизнедеятельностью вашей серверной машины
```
# Информация по системе

# Через бенчмарк

sudo wget -qO- bench.sh | bash 

# Через неофетч (Удобнее чем 1-ый вариант, однако данный вариант не сможет показать скорость)

apt install neofetch

neofetch
```

### IPTables - Закрытие порта SSH, SFTP (22) + ICMP DROP
- На самом деле не рекомендую делать такое с динамическим IP, иначе вы рискуете потерять доступ к вашей серверной машине
```
# Базовые настройки IPTables | Запрет пинга на ваш дедик | Запрет входа с других айпи по SSH (только ваш)

iptables -A INPUT -s IP/32 -p icmp -j DROP

# Или через FirewallD

sudo firewall-cmd --add-icmp-block=echo-request --permanent

# Разрешить свой айпи для входа через SSH,SFTP - ПУНКТ ДЕЛАТЬ ПЕРВЫМ ИЗ ВСЕХ!

iptables -A INPUT -p tcp --dport 22 -s YourIP -j ACCEPT

# Дропнуть порт 22 (aka SSH, SFTP). < ! > ДЕЛАТЬ ПОСЛЕ РАЗРЕШЕНИЯ < ! >

iptables -A INPUT -p tcp --dport 22 -j DROP

# Установка

apt-get install iptables-persistent

# Правила iptables необходимо создать, затем выполнить следующую команду

service iptables-persistent start

# Поскольку утилита является демоном — прекратить ее работу нельзя, однако можно очистить список правил

service iptables-persistent flush

# Обновить правила, если persistent уже был установлен

dpkg-reconfigure iptables-persistent
```

### IPTables - Закрытие портов на нескольких серверных машинах

- Здесь вы подробно узнаете как легко и быстро закрыть порты
```
# Представим что у нас есть два сервера VDS / VPS
# Первый сервер под маркой X - Это у нас будет Proxy сервер (Фильтр различных ботов, пакетов (TCP))
# Второй сервер под маркой Y - Это у нас будет Survival сервер (Основное выживание)
# На сервере X пропишите следующие команды в данном порядке, как они даны (Свреху вниз)

iptables -A INPUT -s <IP Y> -j ACCEPT
iptables -A INPUT -s 127.0.0.1 -j ACCEPT

# На сервере Y пропишите следующие команды в данном порядке, как они даны (Сверху вниз)

iptables -A INPUT -p tcp -s <IP X> --dport <PORT Y (Survival)> -j ACCEPT
iptables -A INPUT -p tcp --dport <PORT Y (Survival)> -j DROP

# После манипуляций на каждом VDS / VPS нужно ввести данную команду

apt install iptables-persistent

# Если у вас он уже был установлен, то просто обновите правила используя команду

dpkg-reconfigure iptables-persistent


# Если хотите можете также ознакомиться со списком ваших правил iptables на каждой из виртуальных машин

iptables --list

# Узнать номера всех правил

iptables -L --line-numbers

# Удалять правила можно следующим способом

iptables -D INPUT ЧИСЛО
```

### "WinRar" - Известный архиватор, но для Linux
```
# Установка
apt install zip unzip

# Там где нужно будет создать архив - у меня это папку /home
cd home

# Архивирование папки/файла | Можно находиться в любом пути (Вы указываете конкретно путь до папки/файла , который нужно заархивировать)
zip -r NAME.zip /home/BungeeCord

# Для примера в моем случае
# /home - дирректория папки с сервером
# /BungeeCord - сама папка с банджей, можно любую например: Survival, Anarchy, SkyBlock.

zip -r surv.zip /home/Survival

# Если имеется SkyBlock папка с сервером, то введите эту команду
# Указывать можно любой сервер, также вы можете например хранить сервер по пути /servers/BungeeCord
# Не обязательно использовать /home раздел для серверов!

zip -r sb.zip /home/SkyBlock

# Либо используйте встроенный tar
```

### MySQL - Для Linux
```
# Установка
sudo apt install mysql-server

# Вход в саму БД
sudo mysql

# Создать БД

CREATE DATABASE IF NOT EXISTS DATABASE_NAME;

# Удалить БД

DROP DATABASE IF EXISTS DATABASE_NAME;

# Список БД

SHOW DATABASES;

# Создать юзера

CREATE USER IF NOT EXISTS 'DATABASE_USER'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'DATABASE_USER_NEW_PASSWORD';

# Изменить пароль юзеру

ALTER USER 'DATABASE_USER'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'DATABASE_USER_NEW_PASSWORD';

# Удалить юзера

DROP USER IF EXISTS 'DATABASE_USER'@'localhost';

# Список юзеров

SELECT user, host FROM mysql.user;

# Выдать права юзеру на определенную БД

GRANT ALL PRIVILEGES ON database_name.table_name TO 'DATABASE_USER'@'localhost';

# Выдать все права юзеру на все БД

GRANT ALL PRIVILEGES ON *.* TO 'DATABASE_USER'@'localhost';

# Список всех прав юзера БД

SHOW GRANTS FOR 'DATABASE_USER'@'localhost';

# Сделать дамп всей БД

mysqldump -uDATABASE_USER -pDATABASE_USER_PASSWORD --all-databases > DATABASE_CUSTOM_DUMP_NAME.sql

# Сделать дамп определенной БД

mysqldump -uDATABASE_USER -pDATABASE_USER_PASSWORD DATABASE_NAME > DATABASE_CUSTOM_DUMP_NAME.sql

# Если вы хотите скопировать ваш дамп БД в другое место

cp /var/lib/mysql/DATABASE_CUSTOM_DAMP_NAME.sql /home/testuser/
```

### Отключение пароля для `$ sudo {cmd}`
```
# Для начала создайте файл (делать от рута ~)

sudo visudo -f /etc/sudoers.d/sudowpass

# В файлик вписываем то что ниже

# You can replace $user
$user ALL=(ALL) NOPASSWD:ALL

```
### Передача файлов по SSH через `scp`
```
scp USER@IP:/путь_для_файла_на_другой_сервер название_файлика_на_вашем_сервере

# У вас запросит пароль, если вы используете ключи, то добавьте -i
```

### Создание пользователя `замена ~ root`
```
# Замените '$name' вашим новым кастомным юзером

adduser $name

# Выдача прав на sudo для нового юзера

usermod -aG sudo $name

```
### Настройка языковых пакетов на Linux
```
sudo dpkg-reconfigure locales

# Вы можете настроить языковые пакеты для себя.
```

# Раздел главных настроек нашего серверного ядра
- Здесь не будет описываться скачивание, запуск сервера и т.д ( google.com )
### Защита вашего сервера
- Не испульзуйте плагины с левых сайтов!
```
Если вы хотите сохранить безопасность вашего сервера и ваших игроков, то лучше всего скачайте оригинальные плагины, а если нужно купить, то лучше купите их.
```

- Закрывайте все ненужные порты, а нужные открывайте!
```
Это конечно не критично, но лучше всего закрыть все порты. После установки firewalld они автоматически закрываются, кроме 22*
```
### Оптимизация сервера
- Пожалуйста не используйте плагины на оптимизацию, в большинстве случаев они грузят сервер сильнее
```
> CleagLagg Plugin - Да, данный плагин может быть полезен для версий 1.12.2 и ниже, но для новых версий он вызывает
куда больше нагрузки, нежели помогает ( PaperMC давно имеет функционал данного плагина* )
Отключение или ограничение вагонеток, арморстендов через данный плагин не спасет вас от крашей.

> Стаккеры мобов в одного моба (Много подобных плагинов) - Даже от них нет особого смысла. 
Пейпер давно позволяет нормально оптимизировать мобов, а если мы используем форки по типу фуги или пурпура, 
то возможностей куда больше для оптимизации вашего игрового сервера

> АвтоОчистка лута на земле - Изучите файлики paper.yml / spigot.yml, но пожалуйста не используйте для этого плагины по типу ClearLagg

> Платный плагин / Самопис от себя или студий - Данное ПО не является аргументом для вашей производительности, 
лучше придерживаться уже известных плагинов, а по вашему желанию вы можете их дополнить, т.к большинство из них OpenSource
```
# О создании игрового сервера в Minecraft
### Рекомендуемое ПО для запуска сервера
- Если вы планируете разрабатывать модовый сервер, то определенно рекомендую __[Fabric](https://fabricmc.net/)__
> Моды можно найти [здесь](https://modrinth.com/mods) или [здесь](https://www.curseforge.com/minecraft/mc-mods)
- Если вы планируете разрабатывать обычный сервер, то определенно рекомендую __[PaperMC](https://papermc.io/) | [PurPur](https://purpurmc.org/)__
> Рекомендуемое ПО Для разработки ___Proxy___ сервера __[Velocity](https://velocitypowered.com/) | [Velocity с сайта PaperMC](https://papermc.io/)__

### VDS/DEDICATED или PANEL HOSTING?
- __Автор__ данного поста не поддерживает панельные хосты из-за их ограничений пользования. Если вы хотите создать качественный Проект, то пожалуйста придерживайтесь использовать выделенных или виртуальных серверо с полным доступом к __SSH__ протоколу

# Об авторе
### Какое ПО используется для разработок игровых проектов (Сервер-сайд)
- Я пользуюсь этими ПО: __[Fabric](https://fabricmc.net/) | [PaperMC](https://papermc.io/) | [PurPur](https://purpurmc.org/) | [Pufferfish](https://ci.pufferfish.host/) | [Velocity](https://velocitypowered.com/)__
> __Pufferfish Links:__ _[Docs](https://docs.pufferfish.host/) | [Github](https://github.com/pufferfish-gg/Pufferfish)_
### Какое ПО используется для разработок игровых клиентов (Клиент-сайд)
- Я пользуюсь этим ПО: __[Fabric](https://fabricmc.net/)__
### Какое ПО используется для подключения к серверу по SSH, SFTP
- Я пользуюсь этими ПО на Windows: [Termius (SSH Полностью бесплано, SFTP Пробная версия, потом платно](https://termius.com/) [WinSCP (SFTP Полностью бесплатно)](https://winscp.net/eng/download.php)
- На Linux использую обычный терминал Linux для `SSH` и FileZilla для `SFTP/FTP` подключений
### На чем создаю Проекты (ПК/Ноут, LINUX OS)
- Для многих это покажется странно, но я разрабатываю все свои Проекты на ноутбуке 
- Работаю в данный момент на Manjaro Linux
