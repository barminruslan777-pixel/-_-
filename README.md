РЕЗЕРВНОЕ КОПИРОВАНИЕ

# Создать папку /backup/
sudo mkdir -p /backup

# Проверить, что папка создалась
ls -la / | grep backup

# Выполнить команду снова
sudo tar -czvf /backup/system_backup_$(date +%Y%m%d_%H%M%S).tar.gz --exclude=/backup --exclude=/proc --exclude=/sys --exclude=/dev --exclude=/tmp --exclude=/run --exclude=/mnt --exclude=/media --exclude=/lost+found /



СОЗДАНИЕ ТОЧЕК ВОССТАНОВЛЕНИЯ ---



создать группы пользователей, настроить права доступа;
настроить аутентификацию и авторизацию, журналов мониторинга;


# 1. Создание групп
sudo groupadd developers 2>/dev/null
sudo groupadd admins 2>/dev/null
sudo groupadd users 2>/dev/null

# 2. Создание пользователей
sudo useradd -m -G developers -s /bin/bash developer1 2>/dev/null
echo "developer1:dev123" | sudo chpasswd

sudo useradd -m -G admins,sudo -s /bin/bash admin1 2>/dev/null
echo "admin1:admin123" | sudo chpasswd

sudo useradd -m -G users -s /bin/bash user1 2>/dev/null
echo "user1:user123" | sudo chpasswd

# 3. Проверка
echo "=== ПРОВЕРКА ==="
groups developer1
groups admin1
groups user1

# 4. Создание папок и прав
echo "=== НАСТРОЙКА ПРАВ ==="
sudo mkdir -p /srv/projects
sudo chown root:developers /srv/projects
sudo chmod 770 /srv/projects

sudo mkdir -p /srv/admin
sudo chown root:admins /srv/admin
sudo chmod 750 /srv/admin

sudo mkdir -p /srv/public
sudo chown root:users /srv/public
sudo chmod 755 /srv/public

# 5. Показать права
ls -la /srv/

# 6. Настройка аутентификации
echo "=== НАСТРОЙКА АУТЕНТИФИКАЦИИ ==="
echo "auth required pam_tally2.so deny=5 unlock_time=60" | sudo tee -a /etc/pam.d/common-auth > /dev/null
echo "%admins ALL=(ALL:ALL) ALL" | sudo tee -a /etc/sudoers.d/admins > /dev/null

# 7. Сохранение логов
echo "=== ЖУРНАЛЫ МОНИТОРИНГА ==="
sudo journalctl --since "2026-06-26 00:00:00" > ~/logs_26.06.2026.txt
sudo last | head -20 > ~/last_logins.txt














Краткая инструкция по работе

📌 ЧАСТЬ 1: ВИРТУАЛЬНАЯ МАШИНА
Создание ВМ:
VirtualBox → Создать
Имя: Ubuntu-16.04-Exam
Тип: Linux → Версия: Ubuntu (64-bit)
RAM: 2048 MB
Диск: VDI → Динамический → 20 GB
Настроить → Носители → подключить ISO-образ Ubuntu 16.04

---------------------------------------------------

Установка Ubuntu:
Язык: Русский
"Стереть диск и установить Ubuntu"
Пользователь: student
Пароль: Password123!

---------------------------------------------------

Гостевые дополнения:
Устройства → Подключить образ диска Гостевых дополнений
Запустить VBoxLinuxAdditions.run
Перезагрузить

Устройства → Общий буфер обмена → Двунаправленный
===================================================================




📌 ЧАСТЬ 2: НАСТРОЙКА ОС
Обновление и SSH:

sudo apt update
sudo apt upgrade -y
sudo apt install openssh-server -y
sudo systemctl status ssh   # проверить

---------------------------------------------------

Интернет:
Настройки сети → DHCP (автоматически)
Проверка: ping google.com

---------------------------------------------------

Виртуальный принтер:
Настройки → Принтеры → Добавить → Virtual PDF Printer
===================================================================




📌 ЧАСТЬ 3: БЕЗОПАСНОСТЬ
Пользователи и группы:
Настройки → Пользователи → Разблокировать

Создать:
1. Создание группы:
sudo groupadd developers
sudo groupadd admins
sudo groupadd users


2. Создание пользователей:
   
# Пользователь developer1 (стандартный)
sudo useradd -m -G developers -s /bin/bash developer1
sudo passwd developer1

# Пользователь admin1 (администратор)
sudo useradd -m -G admins,sudo -s /bin/bash admin1
sudo passwd admin1

# Пользователь user1 (стандартный)
sudo useradd -m -G users -s /bin/bash user1
sudo passwd user1



Права доступа:
sudo mkdir /srv/projects
sudo chown root:developers /srv/projects
sudo chmod 770 /srv/projects

---------------------------------------------------

Резервное копирование:
sudo apt install timeshift -y
timeshift   # запустить GUI
# Создать → RSYNC → Выбрать диск → Создать

Журналы:
Меню → "Журналы" (Logs) → показать логи
===================================================================




📌 ЧАСТЬ 4: УСТАНОВКА ПО

Android Studio:
Скачать .deb с developer.android.com/studio
Дважды кликнуть → Установить

Code::Blocks:
sudo apt install codeblocks codeblocks-contrib -y

GIMP + Inkscape:
sudo apt install gimp inkscape -y

VirtualBox:
sudo apt install virtualbox -y


Антивирус (ClamAV):
sudo apt install clamav clamav-daemon clamtk -y

Запустить ClamTk → Обновить → Сканировать
===================================================================




📌 ЧАСТЬ 5: ДОКУМЕНТАЦИЯ
LibreOffice Writer:
Создать документ по структуре:

text
РУКОВОДСТВО ПОЛЬЗОВАТЕЛЯ
[Название ПО: GIMP]

1. Наименование: GIMP
2. Краткое описание: растровый редактор
3. Запуск: Меню → Графика → GIMP
4. Выход: Файл → Выход (Ctrl+Q)
5. Функции: ретушь, слои, фильтры
6. Приемы: Ctrl+O (открыть), Ctrl+S (сохранить)
Сохранить как .odt и .pdf

Обоснование выбора ПО:
Кратко написать почему выбрали каждую программу:

Android Studio → официальная среда

Code::Blocks → бесплатный, C/C++

GIMP → бесплатный аналог Photoshop

VirtualBox → эмуляция ОС

ClamAV → бесплатный антивирус

LibreOffice → совместимость с MS Office


📌 БЫСТРЫЕ КОМАНДЫ (ШПАРГАЛКА)
bash
# Обновление
sudo apt update && sudo apt upgrade -y

# Установка всего ПО
sudo apt install openssh-server codeblocks gimp inkscape virtualbox clamav clamtk timeshift -y

# Создание папки и прав
sudo mkdir /srv/projects
sudo chown root:developers /srv/projects
sudo chmod 770 /srv/projects

# Проверка SSH
sudo systemctl status ssh

# Запуск Timeshift
timeshift
✅ ЧЕК-ЛИСТ НА ЭКЗАМЕНЕ
Ubuntu установлена и загружается

Интернет работает

SSH включен

Виртуальный принтер установлен

Созданы 3 пользователя

Создана группа developers

Права на папку /srv/projects настроены

Точка восстановления создана (Timeshift)

Android Studio установлена

Code::Blocks установлен

GIMP установлен

VirtualBox установлен

ClamTk установлен и обновлен

Документация (PDF) готова

Обоснование выбора ПО готово
