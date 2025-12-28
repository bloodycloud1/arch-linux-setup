# Pipewire

## 1. Установка звукового сервера
```sudo pacman -S pipewire pipewire-pulse pipewire-alsa pipewire-jack```

## Для управления звуком и Bluetoth, а также для виджетов в баре (Wlroots):
#### wireplumber: обязательный менеджер сессий для управления аудиоустройствами.
#### pamixer / pavucontrol: утилиты для управления громкостью (консоль и GUI).
#### pipewire-pulse: для совместимости с приложениями, ожидающими PulseAudio.
```sudo pacman -S wireplumber pamixer pavucontrol```

#### Для Bluetooth-гарнитур также установите:
```sudo pacman -S pipewire-bluetooth bluez bluez-utils```
#### Не забудьте включить службу Bluetooth: 
```sudo systemctl enable --now bluetooth```


## 2. Настройка PipeWire (если требуется)
Для большинства случаев настройки по умолчанию уже хороши. Если хотите настроить, можно редактировать конфиги в ```/etc/pipewire/``` или ```~/.config/pipewire/```.

## 3. Управление звуком в Hyprland (Ваш hyprland.conf)
В ваш конфиг (```~/.config/hypr/hyprland.conf```) добавьте бинды для управления громкостью:
```# Для pamixer (консоль)```
```bind = SUPER, XF86AudioRaiseVolume, exec, pamixer --increase 5```
```bind = SUPER, XF86AudioLowerVolume, exec, pamixer --decrease 5```
```bind = SUPER, XF86AudioMute, exec, pamixer --toggle-mute```
```# Или для pavucontrol (GUI)```
```# bind = SUPER, XF86AudioRaiseVolume, exec, pavucontrol```

```SUPER``` - это клавиша Windows/Super.<br>
```XF86Audio...``` - это клавиши мультимедиа с клавиатуры.<br>

/* Не проверено
bash
# Громкость вверх
bind = , XF86AudioRaiseVolume, exec, wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%+
# Громкость вниз
bind = , XF86AudioLowerVolume, exec, wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-
# Выключить звук
bind = , XF86AudioMute, exec, wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle
   Не проверено */

## 4. Запуск сервисов PipeWire
Убедитесь, что сервисы PipeWire запущены и включены:
```systemctl --user --now enable pipewire pipewire-pulse wireplumber```
```systemctl --user start pipewire pipewire-pulse wireplumber```

Шаг 4: Проверка и Запуск
Перезапустите Hyprland (выход и вход в систему, или Super + R для перезапуска).
Запустите pavucontrol из терминала.
Вкладка "Вывод" покажет ваши устройства (наушники, колонки). Выберите нужное устройство и установите его как "Устройство по умолчанию".
Проверьте звук, запустив видео или музыку

## 5. Проверка работы
```pactl info```