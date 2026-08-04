# Pipewire

### 1. Установка звукового сервера Pipewire
```sudo pacman -S pipewire pipewire-pulse pipewire-alsa pipewire-jack```

#### wireplumber: обязательный менеджер сессий для управления аудиоустройствами.
#### pamixer / pavucontrol: утилиты для управления громкостью (консоль и GUI).
#### pipewire-pulse: для совместимости с приложениями, ожидающими PulseAudio.
```sudo pacman -S wireplumber pamixer pavucontrol```

### 2. Запуск сервисов PipeWire
Убедитесь, что сервисы PipeWire запущены и включены:<br>
```systemctl --user --now enable pipewire pipewire-pulse wireplumber```<br>
```systemctl --user start pipewire pipewire-pulse wireplumber```<br>

### 3. Настройка PipeWire (если требуется)
Для большинства случаев настройки по умолчанию уже хороши. Если хотите настроить, можно редактировать конфиги в <br>```/etc/pipewire/``` или ```~/.config/pipewire/```

### 4. Управление звуком в Hyprland (Ваш hyprland.conf)
В ваш конфиг (```~/.config/hypr/hyprland.conf```) добавьте бинды для управления громкостью:<br>
```# Для pamixer (консоль)```<br>
```bind = SUPER, XF86AudioRaiseVolume, exec, pamixer --increase 5```<br>
```bind = SUPER, XF86AudioLowerVolume, exec, pamixer --decrease 5```<br>
```bind = SUPER, XF86AudioMute, exec, pamixer --toggle-mute```<br>
```# Или для pavucontrol (GUI)```<br>
```# bind = SUPER, XF86AudioRaiseVolume, exec, pavucontrol```<br>

```SUPER``` - это клавиша Windows/Super.<br>
```XF86Audio...``` - это клавиши мультимедиа с клавиатуры.<br>

#### Громкость вверх
```bind = , XF86AudioRaiseVolume, exec, wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%+```<br>
#### Громкость вниз
```bind = , XF86AudioLowerVolume, exec, wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-```<br>
#### Выключить звук
```bind = , XF86AudioMute, exec, wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle```<br>

### 5. Проверка и Запуск
Перезапустите Hyprland (выход и вход в систему, или Super + R для перезапуска).
Запустите pavucontrol из терминала.
Вкладка "Вывод" покажет ваши устройства (наушники, колонки). Выберите нужное устройство и установите его как "Устройство по умолчанию".
Проверьте звук, запустив видео или музыку.

### 6. Проверка работы
```pactl info```

### 7. Bluetooth

#### Для Bluetooth-гарнитур также установите:
```sudo pacman -S pipewire-bluetooth bluez bluez-utils```

#### Не забудьте включить службу Bluetooth: 
```sudo systemctl enable --now bluetooth```