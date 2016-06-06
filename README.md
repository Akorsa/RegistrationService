# registration_service
Registration service

Registration Service
Технологии которые в обязательном порядке должны использоваться на проекте:
1.	maven
2.	maven-jacoco-plugin с покрытием не меньше чем 85% и встроенным уже в <build> секцию главного проекта
3.	maven-checkstyle-plugin
4.	travis-ci синхронизируемый с вашим github проектом
5.	Spring Boot + Spring JMS
6.	HSQLDB встроенная в сервис при старте и используется как главная база
7.	ActiveMQ как JMS брокер
8.	библиотека Hibernate Validator
9.	Thymeleaf
10.	Skeleton (вместо Bootstrap)
Спокойно: дедлайн 5 июня воскресенье 22-00
Вам необходимо построить микросервис регистрации юзера. Заходим наhttp://localhost:8080/registration

 

Видим справа в углу одинокую кнопку от CSS фрейморка Skeleton, которая тоже будет справа, даже если я открою ее на мобилке с названием "sign up" (регистрировать).
После того как я нажму на нее страница не перезагрузиться (single page design) а просто появятся следующие формы регистрации:

 

Вводим наш email и пароль (который работает по правилам Bean Validation с помощью библиотеки Hibernate Validator - это значит что он будет валидировать эти поля на основе:
1.	классического email regexp
2.	пароля который должен содержать не менее 2 чисел и одного восклицательного знака где угодно в пароле
Нажимаем Create my account ! Слышите этот звук?
В этот момент на HSQLDB уже создана базочка для нас с такой таблицой и свежими данными от юзера который только что заполнил данные.
| email (VARCHAR) | password (VARCHAR) | is_confirmed (BOOLEAN) |
|my_mail@gmail.com| katie44 | false |
Помните, что HSQLDB это in-memory db, после закрытия сервиса все данные должны быть удалены и это условие задачи!
Итак, это данные уже благополучно у вас, а сам юзер видит что у него получилось зарегистрироваться

 

Коварное "check your mail" на странице вверху говорит что ему нужно проверить реально почту для того чтобы потвердить аккаунт. И это не шутки! Ему должен прийти на РЕАЛЬНУЮ почту письмо-потверждение. В этом вам поможет ActiveMQ брокер и Spring JMS (в сети есть туториалы) который пришлет HTML письмо вам на почту в красивом виде.

 

Его ждет письмо что он зарегистрировался, что у него такой то адрес почты и слегка скрытый пароль который обнажает только последние два символа.
Юзер кликает на ссылку. Обрати внимание что у ссылки маппинг на /confirmи некое зашифрованное количество char которое на самом деле содержит информацию о почте и пароле для того чтобы сервис проверил есть ли такая в базе и поставил флажок true в поле is_confirmed
| email (VARCHAR) | password (VARCHAR) | is_confirmed (BOOLEAN) |
|my_mail@gmail.com| katie44 | true |
Финито ля комедия! После того как он поставил флажок на true он перенаправляет вас на /success

 

в котором будет его ждать вот это долгожданное видео:
https://www.youtube.com/watch?v=dQw4w9WgXcQ
Технологически подитожим:
•	maven собирает ваш проект и ваши библиотеки
•	jacoco следит за тем что вы покрываете все тестами
•	checkstyle следит за тем что вы аккуратно пишет код
•	spring boot быстро подымает вам сервис с уже настроенными настройками
•	hibernate validator с помощью аннотаций будет проверять правильно ли юзер вводит почту и пароль
•	hsqldb встроенная in-memory база данных которая подымается вместе в spring boot с уже созданной таблицой без записей. Если сервис закрылся или отвалился, то база сгорает вместе с данными
•	activemq - message брокер который с помощью специальных настроек и поддержки Spring JMS отправим письмо напичканное HTML и информацией на реальную почту
•	skeleton красиво оформит ваш сайт
•	view шаблоном будет выступать thymeleaf
•	travic-ci будет проверять ваши pull request-ы на гитхабе на предмет тестов и чекстайла
Удачи!
