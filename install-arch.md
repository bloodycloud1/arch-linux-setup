# Старт

## 1) Работа со временем
##### Синхронизация времени
```timedatectl set-ntp true```
#####  Установка времени
```timedatectl set-timezpne Asia/Ekaterinburg```
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
Partition number (1-128, default 1): ```1``` 
First sector (2048-87791070, default 2048): ```2048```
Last sector: ```+20G```
#### 2. Второй Partition нужен для остального рабочего пространства
Опять вводим ```n```
Partition number (2-128, default 2):** ```2``` 
First sector (41945088-87791070, default 41945088):** ```41945088```
Last sector: ```+80G```

#### 3. Третий Partition нужен для EFI Загрузчика
Опять вводим ```n```
Partition number (3-128, default 2): ```3```
First sector (62945088-87791070, default 41945088): ```62945088```
Last sector: ```+2G```

#### 4. Третий Partition нужен для SWAP подкачки
Опять вводим ```n```
Partition number (3-128, default 2): ```3```
First sector (64045088-87791070, default 41945088): ```64045088```
Last sector: ```+12G```

#### Далее меняем типы разделов (Partition) (3 и 4 раздел)
##### Редактируем 3 раздел
```t```
Partition number (1-4 , default 4):```3``` 
Partition type or allias (type L to list all): ```1``` (type L to list all) - в контекстом меню через команду ```L``` можно посмотреть все типы разделов

##### Редактируем 4 раздел
```t```
Partition number (1-4 , default 4):```4``` 
Partition type or allias (type L to list all): ```19``` (type L to list all) - в контекстом меню через команду ```L``` можно посмотреть все типы разделов

##### Сохраняем Все настройки
команда ```w```
проверяем результаты разметки ```fdisk -l```
##### Для всех разделов создания файловой системы
```mkfs.fat -F32 /dev/sda3```
```mkswap -F32 /dev/sda4```
```mkswon /dev/sda4```
```mkfs.ext4 /dev/sda1```
```mkfs.ext4 /dev/sda2```

##### Монтируем (Mount). Команда `mount` в Linux используется для подключения (монтирования) файловых систем или внешних устройств, таких как USB-накопители и диски, к дереву каталогов системы.
Монтируем и создаем директорию **/mnt**
```mount /dev/sda1 /mnt```
```mkdir /mnt/efi```
```mkdir /mnt/home```
Проверка создания директории **/mnt**
```cd /mnt```
```ls```- ls (list - посмотреть содержимое текущей директории)
возвращаемся  в root ```cd```
монтируем загрузчик ```mount /dev/sda3 /mnt/efi```
монтируем home ```mount /dev/sda2 /mnt/home```

#### Работа с помощью  pacstrap
pacstrap — это скрипт, используемый в дистрибутиве  Arch Linux  для установки базовых пакетов операционной системы в целевую систему, которая обычно монтируется в директорию  /mnt. Он позволяет быстро установить необходимые компоненты, такие как ядро Linux, и затем настроить систему для первого запуска.
```pacstrap /mnt base linux linux-firmware```

#### Точка монтирования наших разделов
genfstab в Arch Linux — это утилита, которая автоматически генерирует содержимое для файла /etc/fstab путем сканирования текущих точек монтирования и вывода их в формате, совместимом с этим файлом. Это упрощает процесс настройки файловой системы, особенно при установке, позволяя создать файл, который описывает, как монтировать дисковые разделы при загрузке системы.
```genfstab -U /mnt >> /mnt/etc/fstab```
Заходим в **VIM** для  проверки ```vim /mnt/etc/fstab```
чтобы выйти из **VIM** нажимаем на комбинацию горячих ```Shift + :```  выходим командой ```q``` и попадаем опять в root

#### Производим смену директории
```arch-chroot /mnt```
```ln -sf /usr/share/zoneinfo/Asia/Ekaterinburg /etc/localtime```
```ls /usr/share/zoneinfo```
```hwclock --systohc```
```pacman -S vim```
#### Работа с locale.gen в VIM
```vim etc/locale.gen```
```/en_US.UTF-8``` - после того как нашли нужно раскомментировать 
```/ru_RU.UTF-8``` - после того как нашли нужно раскомментировать 
Далее  заходим в VIM клавишей  ```i``` - insert мод 
```backspace```
записываем ```shift + :``` далее ```w```
выходим ```shift + :``` далее ```q```
 ```locale-gen``` - uенерируем locale-gen командой
```echo "LANG=en_US.UTF-8" > /etc/locale.conf```
#### hostname
```vim /etc/hostname```
в vim прописываем свой ```username```
записываем ```shift + :``` далее ```w```
выходим ```shift + :``` далее ```q```
#### hosts
```127.0.0.1    localhost```
```::1          localhost```
```127.0.0.1    username.localdomain   username``` - username указываем свой личный
записываем ```shift + :``` далее ```w```
выходим ```shift + :``` далее ```q```

#### Для root пользователя
```passwd```
Добавляем обычного пользователя с паролем
```useradd -m username```
```passwd username```
Добавляем права на пользователя
```usermod -aG wheel,audio,video,optical,storage username ```
```userdbctl groups-of-user username```

#### Добавляем права Sudo
```pacman -S sudo```

Устанавливаем  GRUB
```pacman -S grub```
```pacman -S efibootmgr dosfstools os-prober m-tools```
Делаем Dual boot с Windows
```vim /etc/defaul/grub```
```GRUB_DISABLE_PROBER=false```  - прописываем в vim в GRUB boot lader configuration
записываем ```shift + :``` далее ```w```
выходим ```shift + :``` далее ```q```

```mkdir /boot/EFI```
```mount /dev/sda3 /boot/EFI```
```grub install --target=x86_64-efi --bootloader-id=grub_uefi --recheck```
```grub-mkconfig -o /boot/grub/grub.cfg```

```sudo pacman -S dhcpcd```
```exit```
```reboot```
```sudo systemctl enable dhcpcd```