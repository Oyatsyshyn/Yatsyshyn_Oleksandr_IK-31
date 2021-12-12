# **Лабораторна робота №4**
---
## Послідовність виконання лабораторної роботи:
#### 1. Для ознайомляння з `Docker` звернувся до документації.
#### 2. Для перевірки чи докер встановлений і працює правильно на віртуальній машині запустітив перевірку версії командою `sudo docker -v > my_work.log`, виведення допомоги командою `sudo docker --help >> my_work.log` та тестовий імедж командою `sudo docker run docker/whalesay cowsay Docker is fun >> my_work.log`. Вивід цих команд перенаправляв у файл `my_work.log` та закомітив його до репозиторію.
#### 3. `Docker` працює з Імеджами та Контейнерами. Імедж це свого роду операційна система з попередньо інстальованим ПЗ. Контейнер це запущений Імедж. Ідея роботи `Docker` дещо схожа на віртуальні машини. Спочатку створив імедж з якого буде запускатись контейнер.
#### 4. Для знайомства з `Docker` створив імедж із `Django` сайтом зробленим у попередній роботі.
1. ##### Оскільки мій проект на `Python` то і базовий імедж також потрібно вибрати відповідний. Використовую команду `docker pull python:3.8-slim` щоб завантажити базовий імедж з репозиторію. Переглядаю створеного вміст імеджа командою `docker inspect python:3.8-slim`
    ##### Перевіряю чи добре встановився даний імедж командою:
    
    ```text
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab4 (master) $ docker images
    REPOSITORY        TAG        IMAGE ID       CREATED       SIZE  
    python            3.8-slim   214d62795dbb   3 weeks ago   122MB
    docker/whalesay   latest     6b362a9f73eb   5 years ago   247MB
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab4 (master) $ 
    ```
2. ##### Створив файл з іменем `Dockerfile` та скопіював туди вміс такого ж файлу з репозиторію викладача.
    ###### Вміст файлу `Dockerfile`:
    ```text
    FROM python:3.8-slim
    
    LABEL author="Bohdan"
    LABEL version=1.0
    
    # оновлюємо систему
    RUN apt-get update && apt-get upgrade -y
    
    # Встановлюємо потрібні пакети
    RUN apt-get install git -y && pip install pipenv
    
    # Створюємо робочу папку
    WORKDIR /lab
    
    # Завантажуємо файли з Git
    RUN git clone https://github.com/BobasB/devops_course.git
    
    # Створюємо остаточну робочу папку з Веб-сайтом та копіюємо туди файли
    WORKDIR /app
    RUN cp -r /lab/devops_course/lab3/* .
    
    # Інсталюємо всі залежності
    RUN pipenv install
    
    # Відкриваємо порт 8000 на зовні
    EXPOSE 8000
    
    # Це команда яка виконається при створенні контейнера
    ENTRYPOINT ["pipenv", "run", "python", "manage.py", "runserver", "0.0.0.0:8000"]
    ```
3. ##### Ознайомився із коментарями та зрозумів структуру написання `Dockerfile`.
4. ##### Змінений`Dockerfile` файл:
    ```text
    FROM python:3.8-slim
    
    LABEL author="Pavlo"
    LABEL version=1.0
    
    # оновлюємо систему
    RUN apt-get update && apt-get upgrade -y
    
    # Встановлюємо потрібні пакети
    RUN apt-get install git -y && pip install pipenv
    
    # Створюємо робочу папку
    WORKDIR /lab
    
    # Завантажуємо файли з Git
    RUN git clone https://github.com/oleksandryatsyshyn/Yatsyshyn_Oleksandr_IK-31.git
    
    # Створюємо остаточну робочу папку з Веб-сайтом та копіюємо туди файли
    WORKDIR /app
    RUN cp -r /lab/Yatsyshyn_Oleksandr_IK-31/Lab3/* .
    
    # Інсталюємо всі залежності
    RUN pipenv install
    
    # Відкриваємо порт 8000 на зовні
    EXPOSE 8000
    
    # Це команда яка виконається при створенні контейнера
    ENTRYPOINT ["pipenv", "run", "python", "manage.py", "runserver", "0.0.0.0:8000"]
    ```
#### 5. Створив власний репозиторій на [Docker Hub](https://cloud.docker.com/repository/registry-1.docker.io/oleksandryatsyshyn/lab4). Для цього залогінився у власний аккаунт на `Docker Hub` після чого перейшов у вкладку Repositories і далі натиснув кнопку `Create new repository`.
#### 6. Виконав білд (build) Docker імеджа та завантажтажив його до репозиторію. Для цього я повинен вказати правильну назву репозиторію та TAG. Оскільки мій репозиторій `oleksandryatsyshyn/lab4` то команда буде виглядати `sudo docker build -t oleksandryatsyshyn/lab4:django .`, де `django` - це тег.
Команда `docker images`:
```text
oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab4 (master) $ docker images
REPOSITORY            TAG        IMAGE ID       CREATED        SIZE
Yatsyshyn_Oleksandr_IK-31   1          34b36be630d2   20 hours ago   345MB
oleksandryatsyshyn/lab4     django     34b36be630d2   20 hours ago   345MB
python                3.8-slim   214d62795dbb   2 weeks ago    122MB
docker/whalesay       latest     6b362a9f73eb   6 years ago    247MB
oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab4 (master) $ 
```
Команда для завантаження на власний репозеторій `docker push oleksandryatsyshyn/lab4:django`.
Посилання на мій [`Docker Hub`](https://cloud.docker.com/repository/registry-1.docker.io/oleksandryatsyshyn/lab4) репозиторій та посилання на [`імедж`](https://hub.docker.com/layers/177240535/oleksandryatsyshyn/lab4/django/images/sha256-410727b1de9c473c7f36270fb70d3791de7856ad73e3cd25603c888ab92c7ee9?context=repo).
#### 7. Для запуску веб-сайту виконав команду `sudo docker run -it --name=django --rm -p 8000:8000 oleksandryatsyshyn/lab4:django`:
```text
oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab4 (master) $ sudo docker run -it --name=django --rm -p 8000:8000 oleksandryatsyshyn/lab4:django
[sudo] password for oleksandryatsyshyn: 
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.
December 12, 2021 - 16:19:31
Django version 3.2.9, using settings 'my_site.settings'
Starting development server at http://0.0.0.0:8000/
Quit the server with CONTROL-C.
[12/Dec/2021 16:19:37] "GET / HTTP/1.1" 200 158

```
Перейшов на адресу http://127.0.0.1:8000 та переконався що мій веб-сайт працює:
#### 8. Оскільки веб-сайт готовий і працює, потрібно створит ще один контейнер із програмою моніторингу нашого веб-сайту (Моє Завдання на роботу):
1. ##### Створив ще один Dockerfile з назвою `Dockerfile.site` в якому помістив програму моніторингу.
    Вміст файлу `Dockerfile.site`:
    ```text
    FROM python:3.8-slim
    
    LABEL author="Pavlo"
    LABEL version=1.0
    
    # оновлюємо систему
    RUN apt-get update && apt-get upgrade -y
    
    # Встановлюємо потрібні пакети
    RUN apt-get install git -y && pip install pipenv
    
    # Створюємо робочу папку
    WORKDIR /lab
    
    # Завантажуємо файли з Git
    RUN git clone https://github.com/oleksandryatsyshyn/Yatsyshyn_Oleksandr_IK-31.git
    
    # Створюємо остаточну робочу папку з Веб-сайтом та копіюємо туди файли
    WORKDIR /app
    RUN cp -r /lab/Yatsyshyn_Oleksandr_IK-31/Lab3/* .
    
    # Інсталюємо всі залежності
    RUN pipenv install
    
    # Відкриваємо порт 8000 на зовні
    EXPOSE 8000

    # Це команда яка виконається при створенні контейнера
    ENTRYPOINT ["pipenv", "run", "python", "monitoring.py", "0.0.0.0:8000"]
    ```
2. ##### Виконав білд даного імеджа та дав йому тег `monitoring` командами:
    ```text
    sudo docker build -f Dockerfile.site -t oleksandryatsyshyn/lab4:monitoring .
    docker push oleksandryatsyshyn/lab4:monitoring
    ```
3. ##### Запустив два контейнери одночасно (у різних вкладках) та переконався що програма моніторингу успішно доступається до сторінок мого веб-сайту.
    ##### Використовуючи команди:
    Запуск серевера:
    ```text
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab4 (master) $ docker run -it --name=django --rm -p 8000:8000 oleksandryatsyshyn/lab4:django
    Watching for file changes with StatReloader
    Performing system checks...
    
    System check identified no issues (0 silenced).
    
    You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
    Run 'python manage.py migrate' to apply them.
    December 12, 2021 - 16:44:04
    Django version 3.2.9, using settings 'my_site.settings'
    Starting development server at http://0.0.0.0:8000/
    Quit the server with CONTROL-C.
    Invalid HTTP_HOST header: '0.0.0.0:8000'. You may need to add '0.0.0.0' to ALLOWED_HOSTS.
    Bad Request: /
    [12/Dec/2021 16:44:30] "GET / HTTP/1.1" 400 60228
    Invalid HTTP_HOST header: '0.0.0.0:8000'. You may need to add '0.0.0.0' to ALLOWED_HOSTS.
    Bad Request: /favicon.ico
    [12/Dec/2021 16:44:30] "GET /favicon.ico HTTP/1.1" 400 60290
    [12/Dec/2021 16:48:42] "GET /health HTTP/1.1" 301 0
    [12/Dec/2021 16:48:42] "GET /health/ HTTP/1.1" 200 340
    [12/Dec/2021 16:49:42] "GET /health HTTP/1.1" 301 0
    [12/Dec/2021 16:49:42] "GET /health/ HTTP/1.1" 200 340
    [12/Dec/2021 16:50:42] "GET /health HTTP/1.1" 301 0
    [12/Dec/2021 16:50:42] "GET /health/ HTTP/1.1" 200 340
    [12/Dec/2021 16:51:42] "GET /health HTTP/1.1" 301 0
    [12/Dec/2021 16:51:42] "GET /health/ HTTP/1.1" 200 340
    [12/Dec/2021 16:52:42] "GET /health HTTP/1.1" 301 0
    [12/Dec/2021 16:52:42] "GET /health/ HTTP/1.1" 200 340
    23:53:19 oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab4 (master) $ 
    ```
    Запуск моніторингу:
    ```text
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab4 (master) $ sudo docker run -it --name=monitoring --rm --net=host -v $(pwd)/server.log:/app/server.log oleksandryatsyshyn/lab4:monitoring
    ^CTraceback (most recent call last):
      File "monitoring.py", line 32, in <module>
        time.sleep(60)
    KeyboardInterrupt
    oleksandryatsyshyn ~/TPIS/Yatsyshyn_Oleksandr_IK-31/Lab4 (master) $ 
    ```
    (перед запуском моніторингу спочатку створив файл server.log)
    Вміст файла `Server.log`:
    ```text
    INFO 2021-12-12 16:48:42,576 root : Сервер доступний. Час на сервері: Time: 16:48:42   Data: 2021/48/11/13/21
    INFO 2021-12-12 16:48:42,577 root : Запитувана сторінка: : localhost:8000/health/
    INFO 2021-12-12 16:48:42,577 root : Відповідь сервера місти наступні поля:
    INFO 2021-12-12 16:48:42,577 root : Ключ: date, Значення: Time: 16:48:42   Data: 2021/48/11/13/21
    INFO 2021-12-12 16:48:42,577 root : Ключ: current_page, Значення: localhost:8000/health/
    INFO 2021-12-12 16:48:42,577 root : Ключ: server_info, Значення: Name_OS: Linux;   Name_Node: 7d4912b37119;   Release: 5.11.0-38-generic;   Version: #42~20.04.1-Ubuntu SMP Tue Sep 28 20:41:07 UTC 2021;   Indentificator:x86_64
    INFO 2021-12-12 16:48:42,577 root : Ключ: client_info, Значення: Browser: python-requests/2.26.0;   IP: 172.17.0.1
    INFO 2021-12-12 16:49:42,649 root : Сервер доступний. Час на сервері: Time: 16:49:42   Data: 2021/49/11/13/21
    INFO 2021-12-12 16:49:42,649 root : Запитувана сторінка: : localhost:8000/health/
    INFO 2021-12-12 16:49:42,649 root : Відповідь сервера місти наступні поля:
    INFO 2021-12-12 16:49:42,649 root : Ключ: date, Значення: Time: 16:49:42   Data: 2021/49/11/13/21
    INFO 2021-12-12 16:49:42,650 root : Ключ: current_page, Значення: localhost:8000/health/
    INFO 2021-12-12 16:49:42,650 root : Ключ: server_info, Значення: Name_OS: Linux;   Name_Node: 7d4912b37119;   Release: 5.11.0-38-generic;   Version: #42~20.04.1-Ubuntu SMP Tue Sep 28 20:41:07 UTC 2021;   Indentificator:x86_64
    INFO 2021-12-12 16:49:42,650 root : Ключ: client_info, Значення: Browser: python-requests/2.26.0;   IP: 172.17.0.1
    INFO 2021-12-12 16:50:42,726 root : Сервер доступний. Час на сервері: Time: 16:50:42   Data: 2021/50/11/13/21
    INFO 2021-12-12 16:50:42,727 root : Запитувана сторінка: : localhost:8000/health/
    INFO 2021-12-12 16:50:42,727 root : Відповідь сервера місти наступні поля:
    INFO 2021-12-12 16:50:42,727 root : Ключ: date, Значення: Time: 16:50:42   Data: 2021/50/11/13/21
    INFO 2021-12-12 16:50:42,727 root : Ключ: current_page, Значення: localhost:8000/health/
    INFO 2021-12-12 16:50:42,727 root : Ключ: server_info, Значення: Name_OS: Linux;   Name_Node: 7d4912b37119;   Release: 5.11.0-38-generic;   Version: #42~20.04.1-Ubuntu SMP Tue Sep 28 20:41:07 UTC 2021;   Indentificator:x86_64
    INFO 2021-12-12 16:50:42,727 root : Ключ: client_info, Значення: Browser: python-requests/2.26.0;   IP: 172.17.0.1
    INFO 2021-12-12 16:51:42,796 root : Сервер доступний. Час на сервері: Time: 16:51:42   Data: 2021/51/12/12/21
    INFO 2021-12-12 16:51:42,796 root : Запитувана сторінка: : localhost:8000/health/
    INFO 2021-12-12 16:51:42,796 root : Відповідь сервера місти наступні поля:
    INFO 2021-12-12 16:51:42,797 root : Ключ: date, Значення: Time: 16:51:42   Data: 2021/51/12/12/21
    INFO 2021-12-12 16:51:42,797 root : Ключ: current_page, Значення: localhost:8000/health/
    INFO 2021-12-12 16:51:42,797 root : Ключ: server_info, Значення: Name_OS: Linux;   Name_Node: 7d4912b37119;   Release: 5.11.0-38-generic;   Version: #42~20.04.1-Ubuntu SMP Tue Sep 28 20:41:07 UTC 2021;   Indentificator:x86_64
    INFO 2021-12-12 16:51:42,798 root : Ключ: client_info, Значення: Browser: python-requests/2.26.0;   IP: 172.17.0.1
    INFO 2021-12-12 16:52:42,871 root : Сервер доступний. Час на сервері: Time: 16:52:42   Data: 2021/52/11/13/21
    INFO 2021-12-12 16:52:42,872 root : Запитувана сторінка: : localhost:8000/health/
    INFO 2021-12-12 16:52:42,873 root : Відповідь сервера місти наступні поля:
    INFO 2021-12-12 16:52:42,873 root : Ключ: date, Значення: Time: 16:52:42   Data: 2021/52/11/13/21
    INFO 2021-12-12 16:52:42,873 root : Ключ: current_page, Значення: localhost:8000/health/
    INFO 2021-12-12 16:52:42,873 root : Ключ: server_info, Значення: Name_OS: Linux;   Name_Node: 7d4912b37119;   Release: 5.11.0-38-generic;   Version: #42~20.04.1-Ubuntu SMP Tue Sep 28 20:41:07 UTC 2021;   Indentificator:x86_64
    INFO 2021-12-12 16:52:42,873 root : Ключ: client_info, Значення: Browser: python-requests/2.26.0;   IP: 172.17.0.1
    ```
4. ##### Закомітив `Dockerfile.site` та результати роботи програми моніторингу запущеної з `Docker` контейнера.
