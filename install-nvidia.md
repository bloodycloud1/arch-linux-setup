# Установки драйвера NVIDIA 3060 Ti на Arch Linux с Hyprland
## 1. Установка драйверов:
### Обновите систему:
```sudo pacman -Syu```
### Установите драйверы и утилиты:
#### nvidia-dkms: Основной пакет драйвера, который будет компилироваться для вашего ядра.
```sudo pacman -S nvidia-utils``` 
#### nvidia-utils: Утилиты и библиотеки.
```sudo pacman -S nvidia-dkms```
#### nvidia-settings: Для графических настроек.
```sudo pacman -S nvidia-settings ```

## 2. Установите пакеты для ускорения (опционально, но рекомендуется для Hyprland)
#### Для лучшей производительности и устранения разрывов (tearing) установите libva-nvidia-driver и vulkan-icd-loader (если еще не установлены)
```sudo pacman -S libva-nvidia-driver vulkan-icd-loader```

## 3. Включите KMS для NVIDIA:
Откройте ```/etc/mkinitcpio.conf``` и добавьте ```nvidia``` и ```nvidia_modeset``` в секцию ```MODULES```:<br>
```MODULES=(nvidia nvidia_modeset nvidia_uvm nvidia_drm)```
Пересоберите initramfs:
```sudo mkinitcpio -P```

## 4. Конфигурация Hyprland (в hyprland.conf):
Откройте файл```~/.config/hypr/hyprland.conf.```<br>
#Найдите раздел ````device``` и настройте его для Nvidia, добавив:

#device {
    name = nvidia
    # Если есть проблемы, попробуйте эти опции:
    # no_cursor_warps = true
    # enable_vaapi = true # Для VA-API кодеков, если нужно
    # enable_vulkan = true # Для Vulkan
#}
#Настройки для Hyprland и NVIDIA
#Используйте 'nvidia' вместо 'auto'
#По умолчанию Hyprland хорошо работает с nvidia, но можно настроить:

#Для лучшей производительности
#Monitor config (пример):
#monitor=DSI-1,1920x1080@144,0x0,1 # Укажите свои мониторы
#Hyprland, чтобы он использовал правильные настройки для вашей карты:
#Для 30-й серии карты лучше использовать VK_ICD=NVIDIA
#Убедитесь, что переменные окружения установлены.

## 5. Проверка: (BASH)
После перезагрузки убедитесь, что драйвер загрузился: ```nvidia-smi```<br>
Проверьте, что Hyprland работает плавно, и нет разрывов изображения<br>