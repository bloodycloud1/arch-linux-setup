# Waybar

## Настройка Waybar для Hyprland состоит из двух основных шагов: размещение файлов конфигурации и редактирование двух файлов — config.jsonc (структура панели) и style.css (её внешний вид). Панель Waybar поддерживает интеграцию из коробкиНастройка Waybar для Hyprland состоит из двух основных шагов: размещение файлов конфигурации и редактирование двух файлов — config.jsonc (структура панели) и style.css (её внешний вид). Панель Waybar поддерживает интеграцию из коробки.

## Установка waybar
```sudo pacman -S waybar```

### 1. Подготовка файлов. 
#### Скопируйте стандартные файлы конфигурации в вашу личную директорию:
```cp /etc/xdg/waybar/config.jsonc ~/.config/waybar/```<br>
```cp /etc/xdg/waybar/style.css ~/.config/waybar/```

### 2. Настройка config.jsonc (Структура модулей)

Откройте файл ```~/.config/waybar/config.jsonc```<br>
В текстовом редакторе. Файл разделен на три блока: расположение и список модулей для левой (modules-left), центральной (modules-center) и правой (modules-right) частей.

#### Пример jsonc
jsonc{
    "layer": "top",
    "position": "top",
    "height": 30,
    "modules-left": ["hyprland/workspaces", "hyprland/window"],
    "modules-center": ["clock"],
    "modules-right": ["pulseaudio", "network", "battery", "tray"],
    
    "hyprland/workspaces": {
        "format": "{icon}",
        "on-click": "activate"
    },
    "clock": {
        "format": "{:%H:%M - %d %b}"
    },
    "pulseaudio": {
        "format": "{volume}% {icon}",
        "format-muted": "Muted",
        "format-icons": ["", "", ""]
    }
}

### 3. Настройка style.css (Оформление)
#### Откройте файл ```~/.config/style.css.```
#### Здесь вы задаете цвета, шрифты и фоны.Пример стилизации: css
* {
    font-family: "FiraCode Nerd Font", sans-serif;
    font-size: 14px;
}

window#waybar {
    background-color: rgba(30, 30, 46, 0.85); /* Полупрозрачный фон */
    color: #cdd6f4;
    border-radius: 10px;
}

#workspaces button {
    padding: 0 10px;
    color: #a6e3a1;
}

#workspaces button.focused {
    background-color: #45475a;
    color: #cba6f7;
    border-radius: 8px;
}

#clock, #pulseaudio, #network, #battery {
    padding: 0 10px;
    margin: 4px 5px;
    background-color: #313244;
    border-radius: 8px;
}

### 4. Автозапуск в Hyprland

#### Чтобы Waybar запускался вместе с сессией Hyprland, добавьте строку в автозагрузку.
#### Откройте ```~/.config/hypr/hyprland.conf и добавьте:```
#### ```exec-once = waybar```

### 5. Применение изменений
```killall waybar && waybar```

### 6.Можно запустить wayarb, чтобы он не висел терминале, вот так: 
```hyprctl dispatch exec waybar```

### 7.Установка шрифтов для waybar
### sudo pacman -S ttf-font-awesome ttf-nerd-fonts-symbols