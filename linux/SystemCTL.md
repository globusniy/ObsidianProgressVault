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
- Так же можно проверить определенные статусы, например чтобы проверить активен ли (работает ли) модуль в данный момент можно использовать команду `is-active` `systemctl is-active application.service` Это вернет текущий статус модуля, который обычно `active` или `inactive`. Код выхода будет «0», если он активен, и результат будет проще парсить в скрипты оболочки.
- Чтобы увидеть, включен ли модуль, можно использовать команду `is-enabled` 
- `systemctl is-enabled application.service`
Это выведет информацию о том, что служба `enabled` или `disabled`, и снова установит код выхода на «0» или «1» в зависимости от вопроса команды.



9)



10) Список всех юнитов `systemctl list-units`
