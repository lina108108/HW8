
Тестирование входа в систему интернет-банка через вэб-интерфейс на базе готовой схемы БД.

Начало работы :

- создать проект на базе gradle
- сделать git init в папке проекта и связать удаленный репозиторий с локальным



Prerequisites:

Перед началом работы убедитесь, что на вашем ПК установлены:

- Git

- IntelliJ IDEA CE с поддержкой Java

- Docker Desktop

- Браузер Google Chrome

Окружение:             Mac OS version 10.13.16 ;
                       Java 11.0.7;
                       Chrome 84

Установка и запуск:

Вам нужно запустить docker контейнер. Все параметры для запуска контейнера уже прописаны в docker-compose.yml. Volumes настроены таким образом, что sql схема запустится и на вашей локальной машине. Для запуска используйте команду docker-compose up.

 Дождитесь вывода строки в лог (сообщение о том, что плагин готов к работе):
 
[Server] X Plugin ready for connections. Socket: '/var/run/mysqld/mysqlx.sock' bind-address: '::' port: 33060

Теперь устанавливаем связь с сервером.
 
 Для этого запускаем в терминале команду docker-compose exec mysql mysql -u app -p app -v
 
 В появляющееся поле запроса пароля вводим пароль: pass
 
 Проверить то, что нужные таблицы создались, можно с помощью команды show tables в терминале. 

Запускаем SUT:

Для этого используем команду java -jar ./artifacts/app-deadline.jar

Обратите внимание на указание директории в команде - без этого приложение не будет найдено. Ответом будет запуск SUT и указание на каком порте она запущена. Например, в нашем случае это порт 9999, что и отражено в логах:

2020-08-06 17:54:27.803 [main] TRACE Application - {

    # application.conf @ jar:file:/Users/macbookpro/Downloads/hw8/artifacts/app-deadline.jar!/application.conf: 6
    "application" : {
        # application.conf @ jar:file:/Users/macbookpro/Downloads/hw8/artifacts/app-deadline.jar!/application.conf: 7
        "modules" : [
            # application.conf @ jar:file:/Users/macbookpro/Downloads/hw8/artifacts/app-deadline.jar!/application.conf: 7
            "ru.netology.aqa.ApplicationKt.module"
        ]
    },
    # application.conf @ jar:file:/Users/macbookpro/Downloads/hw8/artifacts/app-deadline.jar!/application.conf: 2
    "deployment" : {
        # application.conf @ jar:file:/Users/macbookpro/Downloads/hw8/artifacts/app-deadline.jar!/application.conf: 3
        "port" : 9999
    },
    
    # Content hidden
    "security" : "***"
}



Теперь запускаем тесты командой ./gradlew clean test 

Они должны проходить.

Если система не запускается или тесты не проходят - создавайте баг-репорт.

Если возникла необходимость повторного запуска тестов - перезапустите приложение вручную. Для этого можно просто закрыть окно терминала с запущенным приложением и кликнуть на кнопку "Terminate" во всплывающем окне. Далее запустите приложеие снова командой java -jar ./artifacts/app-deadline.jar. Контейнер и базу перезапускать при этом нет необходимости - в тестах заложен метод очистки таблиц, чтобы SUT запускался повторно.

