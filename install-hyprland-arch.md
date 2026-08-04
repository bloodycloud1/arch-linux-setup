# Hyprland: Сам композитор.
```sudo pacman -S hyprland```<br>

#### Установка и включение ssdm - менеджер дисплеев 
```sudo pacman -S sddm```<br>
```sudo systemctl enable sddm.service```<br>

#### Установка терминала kitty
```sudo pacman -S kitty``` - терминал<br>

#### Установка thunar файловый менеджер
```sudo pacman -S thunar```

#### Установка wofi: Лаунчер приложений
```sudo pacman wofi```

#### Установка xdg-desktop-portal-hyprland
```xdg-desktop-portal-hyprland```

#### Установка waybar
```sudo pacman -S waybar```

#### Установка hyprpaper
```sudo pacman -S hyprpaper```

#### Установка hyprpolkitagent и включение службы через systemd:
```sudo pacman -S hyprpolkitagent```
```systemctl --user enable --now hyprpolkitagent```

#### Установка mako
```sudo pacman -S mako```

#### Установка hyprlauncher
```sudo pacman -S hyprlauncher```

#### Установка wl-copy clipboard 
```sudo pacman -S wl-clipboard```

#### Установка yay
```git clone https://aur.archlinux.org/yay.git```
```cd yay```
```sudo pacman -S base-devel```
```makepkg -si```