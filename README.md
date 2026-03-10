# Instructions
Cachy OS Setup &amp; Tweaks Guide
📦 Полезные команды pacman

Поиск пакета в репозитории по части названия

pacman -Ss <название>

Поиск среди установленных пакетов

pacman -Qs <название>

Показать пакеты, установленные не через pacman (AUR и др.)

pacman -Qm

Удаление пакета вместе со всеми неиспользуемыми зависимостями

sudo pacman -Rsn minetest
📦 Удаление пакетов через yay

Удаление пакета и зависимостей:

yay -Rsn minetest
📦 Установка менеджера пакетов AUR (yay)

Установка зависимостей:

sudo pacman -S --needed git base-devel

Клонирование и установка:

git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
🌡 Установка и настройка сенсоров
Установка пакета
sudo pacman -S lm_sensors
Загрузка модуля nct6687 (для PWM control)
echo 'nct6687' | sudo tee /etc/modules-load.d/nct6687.conf
Пересборка initramfs
sudo mkinitcpio -P
reboot
Проверка
sensors
🎮 Менеджмент питания GPU
Установка из AUR
yay -S cyan-skillfish-governor-smu
Управление сервисом
sudo systemctl [команда] cyan-skillfish-governor-smu.service

Возможные команды:

start

stop

enable

disable

⚙️ Конфигурационный файл GPU
[[safe-points]]
frequency = 350
voltage = 700

[[safe-points]]
frequency = 800
voltage = 750

[[safe-points]]
frequency = 1200
voltage = 800

[[safe-points]]
frequency = 1700
voltage = 900

[[safe-points]]
frequency = 1850
voltage = 920

[[safe-points]]
frequency = 2000
voltage = 945

[[safe-points]]
frequency = 2050
voltage = 975

[[safe-points]]
frequency = 2100
voltage = 1000
⚡ Менеджмент питания CPU
Установка зависимостей

После этого этапа может потребоваться перезагрузка.

yay -S stress
yay -S python-pipx
pipx ensurepath
pipx --version
Установка SMU менеджера
git clone https://github.com/bc250-collective/bc250_smu_oc.git
cd bc250_smu_oc
pipx install .
Проверка установки
bc250-detect --help
Тестовая настройка частоты и напряжения

(ключ -k добавляется только после предварительного прогона)

bc250-detect --frequency 3850 --vid 1180 --temp 85 -k
Стресс-тест CPU (5 минут)
stress --cpu 16 --timeout 300
Применение конфигурации
bc250-apply --install overclock.conf
Управление сервисом
sudo systemctl [команда] --now bc250-smu-oc

Команды:

start

stop

enable

disable

📡 Настройка USB Wi-Fi / Bluetooth адаптера (Китай)
Добавить правило udev

Добавить строку в файл:

/usr/lib/udev/rules.d/40-usb_modeswitch.rules
ATTR{idVendor}=="1111", ATTR{idProduct}=="1111", RUN+="usb_modeswitch '/%k'"
Создать файл переключения режима

Создать файл:

/usr/share/usb_modeswitch/1111:1111

С содержимым:

TargetVendor=0xa69c
TargetProduct=0x8d80
MessageContent="555342438765432100000000000010fd0000000000000000000000000000f2"
Установка драйвера

Клонирование репозитория и подготовка скрипта:

git clone https://github.com/shenmintao/aic8800d80.git
cd aic8800d80
git checkout bluetooth
chmod +x install.sh
Запуск установки
sudo ./install.sh
