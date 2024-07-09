**systemctl** — это утилита командной строки, которая позволяет взаимодействовать с **systemd**. Она предоставляет команды для управления службами и юнитами **systemd**.
> Про **systemd** говорилось в [[Уровни  загрузки]] пункт 5

`systemctl`, является инструментом центрального управления для контроля системы инициализации **systemd** 

**systemd** управляет различными компонентами системы, используя концепцию "юнитов" (units). Существует несколько типов юнитов:

- **Service** (.service): Определяет службу, например, веб-сервер.
- **Socket** (.socket): Описывает сокет, который ожидает соединений.
- **Target** (.target): Группирует другие юниты для достижения определенного состояния.
- **Mount** (.mount): Управляет точками монтирования файловых систем.
- И другие.
### Основные команды systemctl
1) Запуск службы `sudo systemctl start имя_службы.service`

2) Остановка службы `sudo systemctl stop имя_службы.service`

3) Перезапуск службы `sudo systemctl restart имя_службы.service`
- Останавливает и затем снова запускает указанную службу.

4) Перезагрузка службы, если она уже запущена `sudo systemctl reload имя_службы.service`
- Перезагружает конфигурацию службы без её остановки. 
- Если вы не уверены, есть ли у службы функция перезагрузки своей конфигурации, можно использовать команду `reload-or-restart`. 
- `sudo systemctl reload-or-restart имя_службы.service`

5) sudo systemctl reload-or-restart application.service

6) Включение автозагрузки службы `sudo systemctl enable имя_службы.service`

7) Отключение службы при загрузке `sudo systemctl disable имя_службы.service`

8) Проверка статуса службы `systemctl status имя_службы.service`
- Выводит информацию о текущем состоянии службы 
Например, при проверке статуса сервера Nginx вы можете видеть следующий вывод:
````
Output● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2015-01-27 19:41:23 EST; 22h ago
 Main PID: 495 (nginx)
   CGroup: /system.slice/nginx.service
           ├─495 nginx: master process /usr/bin/nginx -g pid /run/nginx.pid; error_log stderr;
           └─496 nginx: worker process
Jan 27 19:41:23 desktop systemd[1]: Starting A high performance web server and a reverse proxy server...
Jan 27 19:41:23 desktop systemd[1]: Started A high performance web server and a reverse proxy server.
````
- Так же можно проверить определенные статусы, например чтобы проверить активен ли (работает ли) модуль в данный момент можно использовать команду `is-active` `systemctl is-active имя_службы.service` Это вернет текущий статус модуля, который обычно `active` или `inactive`. 
- Чтобы увидеть, включен ли модуль, можно использовать команду `is-enabled` 
`systemctl is-enabled имя_службы.service`
Это выведет информацию о том, что служба `enabled` или `disabled`, и снова установит код выхода на «0» или «1» в зависимости от вопроса команды.

Третья проверка заключается в проверке того, находится ли модуль в состоянии сбоя. Это означает, что была проблема, которая запустила данный модуль 
`systemctl is-failed имя_службы.service`
Это вернет `active`, если он работает должным образом, или `failed`, если возникла ошибка. Если модуль был намеренно остановлен, может вернуться `unknown` или `inactive`.

### Обзор состояния системы

1) Список всех юнитов `systemctl list-units` аналогично `systemctl`
Результат будет выглядеть примерно так:
````
OutputUNIT                                LOAD   ACTIVE SUB     DESCRIPTION
atd.service                               loaded active running ATD daemon
avahi-daemon.service                      loaded active running Avahi mDNS/DNS-SD Stack
dbus.service                              loaded active running D-Bus System Message Bus
dcron.service                             loaded active running Periodic Command Scheduler
dkms.service                              loaded active exited  Dynamic Kernel Modules System
getty@tty1.service                        loaded active running Getty on tty1
. . .
````

Вывод содержит следующие столбцы:

- **UNIT**: имя модуля `systemd`
- **LOAD**: указывает на то, парсила ли `systemd` конфигурацию модуля. Конфигурация загруженных модулей сохраняется в памяти.
- **ACTIVE**: краткое состояние активности модуля. Обычно это довольно стандартный способ сообщить, запущен модуль или нет.
- **SUB**: это состояние более низкого уровня, которое указывает более подробную информацию о модуле. Это часто зависит от типа модуля, состояния и фактического метода работы модуля.
- **DESCRIPTION**: краткое текстовое описание того, чем является модуль/что делает.

Например, чтобы увидеть все модули, которые загрузила система `systemd` (или пыталась загрузить), независимо от их активности в данный момент, можно использовать следующий флаг `--all` `systemctl list-units --all` 
также можно отфильтровать вывод этой команды флагом `--state=` и после равно без пробела указывается состояние которое мы хотим увидеть **LOAD, ACTIVE, SUB**
#### LOAD
- `loaded` — юнит загружен в память.
- `not-found` — юнит не найден.
- `error` — произошла ошибка при загрузке юнита.
- `masked` — юнит замаскирован и не может быть запущен.
#### ACTIVE 
- `active` — юнит активен.
- `reloading` — юнит находится в процессе перезагрузки конфигурации.
- `inactive` — юнит неактивен.
- `failed` — запуск юнита завершился с ошибкой.
- `activating` — юнит находится в процессе активации.
- `deactivating` — юнит находится в процессе деактивации.
#### SUB
- Для сервисов:
    
    - `running` — сервис работает.
    - `exited` — сервис завершился успешно.
    - `dead` — сервис не работает.
    - `start` — сервис находится в процессе запуска.
    - `stop` — сервис находится в процессе остановки.
    - `auto-restart` — сервис автоматически перезапускается.
    - `failed` — запуск сервиса завершился с ошибкой.
- Для сокетов:
    
    - `listening` — сокет прослушивается.
    - `running` — сокет работает.
    - `failed` — сокет не работает.
- Для таймеров:
    
    - `waiting` — таймер ожидает срабатывания.
    - `elapsed` — таймер сработал.
    - `failed` — таймер не сработал.
- Для устройств:
    
    - `plugged` — устройство подключено.
    - `dead` — устройство неактивно.
- Для монтирования:
    
    - `mounted` — файловая система смонтирована.
    - `mounting` — файловая система монтируется.
    - `unmounting` — файловая система отмонтируется.
    - `dead` — файловая система неактивна.

`systemctl list-units --all --state=inactive`
Ответ будет таким:
```
$ systemctl list-units --all --state=inactive
  UNIT                               LOAD      ACTIVE   SUB      DESCRIPTION
  apache2.service                    loaded    inactive dead     The Apache HTTP Server
  systemd-fsckd.service              loaded    inactive dead     File System Check Daemon
  systemd-readahead-collect.service  loaded    inactive dead     Collect Read-Ahead Data
  systemd-readahead-replay.service   loaded    inactive dead     Replay Read-Ahead Data
  udisks.service                     loaded    inactive dead     Storage Daemon
  upower.service                     loaded    inactive dead     Daemon for power management
  ...

LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.
```

Другим распространенным фильтром является `--type=` так можно отсортировать только интересующего нас типа `systemctl list-units --type=service` 
Вывод будет таким:
```
systemctl list-units --type=service
UNIT                             LOAD   ACTIVE SUB     DESCRIPTION
dbus.service                     loaded active running D-Bus System Message Bus
getty@tty1.service               loaded active running Getty on tty1
network.service                  loaded active running Network Service
sshd.service                     loaded active running OpenSSH Daemon
systemd-journald.service         loaded active running Journal Service
systemd-logind.service           loaded active running Login Service
systemd-networkd.service         loaded active running Network Service
systemd-resolved.service         loaded active running Network Name Resolution
systemd-timesyncd.service        loaded active running Network Time Synchronization
udisks2.service                  loaded active running Disk Manager
...
LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.
```

## Список всех файлов модулей
Чтобы увидеть _все_ доступные файлы модулей в путях `systemd`, включая те, что система `systemd` пыталась загрузить, можно использовать команду `list-unit-files`: 
`systemctl list-unit-files` 

Модули являются представлениями ресурсов, о которых знает `systemd`. Поскольку система `systemd` необязательно считывала все определения модуля в этом виде, она представляет информацию только о самих файлах. Вывод содержит два столбца: файл модуля и состояние.
```
$ systemctl list-unit-files
UNIT FILE                             STATE   
apache2.service                       enabled 
cron.service                          enabled 
dbus.service                          static  
getty@.service                        enabled 
networking.service                    enabled 
rescue.service                        static  
sshd.service                          enabled 
syslog.service                        static  
systemd-journald.service              static  
...
```
Состояние будет, как правило, `enabled`, `disabled`, `static` или `masked`
### enabled

- **Описание**: Юнит включен. Это значит, что он автоматически запускается при загрузке системы или при старте соответствующего таргета
### disabled

- **Описание**: Юнит отключен. Это значит, что он не будет автоматически запускаться при загрузке системы или старте соответствующего таргета.
### static

- **Описание**: обозначает, что файл модуля не содержит раздел `install`, который используется для включения модуля. Эти модули как таковые не могут быть включены.
### masked

- **Описание**: Юнит замаскирован. Это значит, что он не может быть запущен ни вручную, ни автоматически.

## Отображение файла модуля
Чтобы отобразить файл модуля, который система `systemd` загрузила в систему, можно использовать команду `cat`
`systemctl cat atd.service`
```
Output[Unit]
Description=ATD daemon
[Service]
Type=forking
ExecStart=/usr/bin/atd
[Install]
WantedBy=multi-user.target
```
Вывод — это файл модуля, известный выполняемому в настоящее время процессу `systemd`.

## Отображение зависимостей
 Чтобы увидеть дерево зависимостей модуля, можно использовать команду 
 `list-dependencies`
 `systemctl list-dependencies sshd.service`
 При этом отобразится иерархическая схема зависимостей, с которой необходимо работать, чтобы запустить интересуемый модуль. Зависимости в этом контексте включают те модули, которые либо требуются, либо желательны для модулей выше.
 ```
Output
sshd.service
├─system.slice
└─basic.target
  ├─microcode.service
  ├─rhel-autorelabel-mark.service
  ├─rhel-autorelabel.service
  ├─rhel-configure.service
  ├─rhel-dmesg.service
  ├─rhel-loadmodules.service
  ├─paths.target
  ├─slices.target
. . .
```
