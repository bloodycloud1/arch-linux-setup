# Старт

## 1) Работа со временем
##### Синхронизация времени
```timedatectl set-ntp true```
#####  Установка времени
```timedatectl set-timezone Asia/Yekaterinburg```
##### Проверка статуса времени
```timedatectl status```
##  2) Работа с дисками
##### Просмотр дисков
```fdisk -l ```
#####  Утилита для разметки разделов диска
```fdisk /dev/sda```
```m``` - команда для просмотра всех доступных команд
```g``` - команда для создания для создания GPT разделов диска
```n``` - команда для создания нового раздела
После команды ```n``` идет настройка диска (то что находится в default можно не вписывать дефолтные числа если не менять их не нужно, а просто нажимать Enter)
#### 1. Первый Partiotion нужен для самой системы Arch Linux
Partition number (1-128, default 1): ```1``` <br>
First sector (2048-87791070, default 2048): ```2048```<br>
Last sector: ```+20G```<br>
#### 2. Второй Partition нужен для остального рабочего пространства
Опять вводим ```n```<br>
Partition number (2-128, default 2):** ```2``` <br>
First sector (41945088-87791070, default 41945088):** ```41945088```<br>
Last sector: ```+90G```

#### 3. Третий Partition нужен для EFI Загрузчика
Опять вводим ```n```<br>
Partition number (3-128, default 2): ```3```<br>
First sector (62945088-87791070, default 41945088): ```62945088```<br>
Last sector: ```+2G```

#### 4. Третий Partition нужен для SWAP подкачки
Опять вводим ```n```<br>
Partition number (3-128, default 2): ```3```<br>
First sector (64045088-87791070, default 41945088): ```64045088```<br>
Last sector: ```+8G```

#### Далее меняем типы разделов (Partition) (3 и 4 раздел)
##### Редактируем 3 раздел
```t```
Partition number (1-4 , default 4):```3```<br> 
Partition type or allias (type L to list all): ```1``` (type L to list all) - в контекстом меню через команду ```L``` можно посмотреть все типы разделов

##### Редактируем 4 раздел
```t```
Partition number (1-4 , default 4):```4```<br> 
Partition type or allias (type L to list all): ```19``` (type L to list all) - в контекстом меню через команду ```L``` можно посмотреть все типы разделов

##### Сохраняем Все настройки
команда ```w```
проверяем результаты разметки ```fdisk -l```
##### Для всех разделов создания файловой системы
```mkfs.fat -F32 /dev/sda3```<br>
```mkswap /dev/sda4```<br>
```swapon /dev/sda4```<br>
```mkfs.ext4 /dev/sda1```<br>
```mkfs.ext4 /dev/sda2```<br>

##### Монтируем (Mount). Команда `mount` в Linux используется для подключения (монтирования) файловых систем или внешних устройств, таких как USB-накопители и диски, к дереву каталогов системы.
Монтируем и создаем директорию **/mnt** <br>
```mount /dev/sda1 /mnt```<br>
```mkdir /mnt/efi```<br>
```mkdir /mnt/home```<br>
##### Проверка создания директории **/mnt**
```cd /mnt```<br>
```ls```- ls (list - посмотреть содержимое текущей директории)
возвращаемся  в root ```cd```<br>
монтируем загрузчик ```mount /dev/sda3 /mnt/efi```<br>
монтируем home ```mount /dev/sda2 /mnt/home```

#### Работа с помощью  pacstrap
pacstrap — это скрипт, используемый в дистрибутиве  Arch Linux  для установки базовых пакетов операционной системы в целевую систему, которая обычно монтируется в директорию  /mnt. Он позволяет быстро установить необходимые компоненты, такие как ядро Linux, и затем настроить систему для первого запуска.<br>
```pacstrap /mnt base linux linux-firmware```

#### Точка монтирования наших разделов
genfstab в Arch Linux — это утилита, которая автоматически генерирует содержимое для файла /etc/fstab путем сканирования текущих точек монтирования и вывода их в формате, совместимом с этим файлом. Это упрощает процесс настройки файловой системы, особенно при установке, позволяя создать файл, который описывает, как монтировать дисковые разделы при загрузке системы.<br>
```genfstab -U /mnt >> /mnt/etc/fstab```<br>
Заходим в **VIM** для  проверки ```vim /mnt/etc/fstab```<br>
чтобы выйти из **VIM** нажимаем на комбинацию горячих ```Shift + :```  выходим командой ```q``` и попадаем опять в root

#### Производим смену директории
```arch-chroot /mnt```<br>
```ln -sf /usr/share/zoneinfo/Asia/Yekaterinburg /etc/localtime```<br>
```ls /usr/share/zoneinfo```<br>
```hwclock --systohc```<br>
#### Устанавливаем VIM
```pacman -S vim```

#### Работа с locale.gen в VIM
```vim etc/locale.gen```<br>
```/en_US.UTF-8``` - после того как нашли нужно раскомментировать<br>
```/ru_RU.UTF-8``` - после того как нашли нужно раскомментировать<br>
Далее  заходим в VIM клавишей  ```i``` - insert мод <br>
```backspace```<br>
записываем ```shift + :``` далее ```w```<br>
выходим ```shift + :``` далее ```q```<br>
 ```locale-gen``` - генерируем locale-gen командой<br>
```echo "LANG=en_US.UTF-8" > /etc/locale.conf```

#### Hostname (имя PC)
```vim /etc/hostname```
в vim прописываем свой ```{username}```<br>
записываем ```shift + :``` далее ```w```<br>
выходим ```shift + :``` далее ```q```<br>
#### Hosts
```127.0.0.1    localhost```<br>
```::1          localhost```<br>
```127.0.0.1    {username}.localdomain   {username}``` - username указываем свой личный<br>
записываем ```shift + :``` далее ```w```<br>
выходим ```shift + :``` далее ```q```<br>

#### Для root пользователя
```passwd```<br>
Добавляем обычного пользователя с паролем
```useradd -m {username}```<br>
```passwd {username}```<br>
Добавляем права на пользователя <br>
```usermod -aG wheel,audio,video,optical,storage {username}```<br>
```userdbctl groups-of-user {username}```

#### Добавляем права Sudo
```pacman -S sudo```<br>
```EDITOR=vim```
```visudo``` <br>
расскоментируем строку ```%wheel ALL=(ALL)```

#### Устанавливаем  GRUB
```pacman -S grub``` <br>
```pacman -S efibootmgr dosfstools os-prober mtools```<br>
#### Делаем Dual boot с Windows
```vim /etc/defaul/grub``` <br>
```GRUB_DISABLE_PROBER=false```  - прописываем в vim в GRUB boot loader configuration<br>
записываем ```shift + :``` далее ```w```<br>
выходим ```shift + :``` далее ```q```<br>

```mkdir /boot/EFI```<br>
```mount /dev/sda3 /boot/EFI```<br>
```grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck```<br>
```grub-mkconfig -o /boot/grub/grub.cfg```<br>

#### Инструмент который позволит настроить интернет 
Dynamic Host Configuration Protocol Client Daemon
```sudo pacman -S dhcpcd```<br>
```exit```<br>
```reboot```<br>
```sudo systemctl enable dhcpcd```<br>

##### Редактируем pacman.conf
```sudo vim /etc/pacman.conf ```<br>
внутри расскоментируем<br>
```#multilib```<br>
```#Include = /etc/pacman.d/mirrorlist```

##### Перезапускаем систему
```exit```<br>
```reboot```<br>
```sudo pacman -Sy```

###### Устанавливаем шрифт
```sudo pacman -S ttf-jetbrains-mono```

######