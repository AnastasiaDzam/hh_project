# Exam: Управление персоналом

### Введение
Необходимо создать MVP приложения управления персоналом. 
Пользователи твоего приложения смогут приглашать людей на собеседование, оставлять комментарии к анкетам, нанимать и увольнять сотрудников.

Делай git push и Pull Request только один раз (к 16.00), в течение экзамена ничего пушить не нужно!
При этом делать commit можно и нужно для каждого релиза.

Сначала прочитай всё задание целиком!

Если не получается сделать какой-то релиз, переходи к следующему.

### Release 0: Настройки
Убедись, что в твоем проекте есть файл .gitignore, и установлены все необходимые библиотеки в папках client и server. Создай базу данных. Проверь наличие файлов: .env и .env.example, .sequelizerc.

### Release 1: Регистрация пользователя
Первое, что нужно сделать — регистрация пользователей. Не забудь про шифрование паролей. Ты можешь использовать для этого [sha256](https://www.npmjs.com/package/sha256) или более продвинутую библиотеку [bcrypt](https://www.npmjs.com/package/bcrypt). 
Называй свои маршруты в архитектурном стиле REST API. 
Тебе поможет вот эта статья на Хабре: [REST API Best Practices](https://habr.com/post/351890/).

В приложении имеются следующие роли:
- Директор
- Менеджер по набору персонала

Изначально роли должны быть созданы при настройке проекта (при инициализации необходимо запустить миграции для создания таблиц в БД, а затем выполнить засеивание базы двумя ролями)

Добавь в компонент Navbar ссылки на регистрацию и на домашнюю страницу. Реализуй регистрацию. 
У каждого пользователя обязательными полями будут: name, email, role и password.
При регистрации пользователь выбирает в выпадающем меню свою роль в приложении.
![registration mockup](readme-assets/2.PNG)  
*Рисунок 1*. Форма регистрации.

### Release 2: Авторизация и выход из приложения
Добавь в компонент Navbar кнопку Login, ведущую на соответствующую страницу. Не забудь обрабатывать ошибки: если у пользователя не получается авторизоваться — ему нужно знать причину. 
При успешном входе вместо кнопки Login должна появляться Logout. При нажатии на Logout пользователь выходит из системы и оказывается на домашней странице. На данном этапе домашняя страница может быть пустой, позже на ней будут отображаться количество сотрудников принятых на работу и ожидающих собеседования.
![login/logout animation](readme-assets/3.PNG)  
*Рисунок 2*. Login, logout.

### Release 3: Менеджер по набору персонала / HR 
Теперь сделай функционал для авторизованных пользователей. Для начала реализуем функционал для роли Менеджер по набору персонала. Если, пользователь авторизован как Менеджер по набору персонала, то происходит редирект на '/'. 
Только для этой роли в компоненте Navbar должна появиться кнопка Пригласить на собеседование нажав которую, из базы данных должны быть возвращены 10 рабочих/workers, и их карточки должны отобразиться в разделе Ожидающие собеседования.

Для этого необходимо реализовать соответствующую модель Worker миграции и сиды в базе данных. Модель Worker должна иметь следующие поля: name, email, phone, image, а так же подумай, как реализовать поле которое отвечало бы за то, что его приняли на работу.

Т.к. картики рабочего нет, реализуй картинку "заглушку" для рабочих без аватарки.

Функционал:
- При нажатии на кнопку Принять карточка пользователя начинает отображаться в разделе Принятые сотрудники, а в базе данных сущность конкретного рабочего меняется так как он теперь является трудоустроеным.
- Кнопка Отклонить удаляет рабочего из базы данных и карточка перестает отображаться на клиенте.

![hr mockup](readme-assets/4.PNG)  
*Рисунок 4*. Интерфейс менеджера.

### Release 4: Директор / Director
Если, пользователь вошел с ролью Директор, то на карточках отображенных в разделе Принятые сотрудники появляется кнопка Уволить, нажатиe на которую удаляет рабочего из базы данных и карточка перестает отображаться. Функциональные кнопки Менеджера по набору персонала роли Директор недоступны.

Asel, [25.11.2024 16:34]
![director mockup](readme-assets/5.PNG)  
*Рисунок 4*. Интерфейс директора.

### Release 5: Главная страница / Main page
Если, в приложении никто не авторизован, то отображается количество рабочих принятых и ожидающих собеседование.
![401 mockup](readme-assets/1.PNG)  
*Рисунок 1*. Не авторизованный пользователь.

### Release 6: Соответствие поведений пользователей / Appropriate Behaviors per User and Route
Проанализируйте функциональность, которую вы уже реализовали. 
Все ли сделано верно с точки зрения логики, распределения ролей, удобства пользователя, безопасности?

* Каждый ли пользователь видит функциональные кнопки, или только те, кто вошел в систему?
* Могут ли неавторизованные пользователи получить доступ к карточкам сотрудников ?
* Можно ли редактировать чужие комментарии?

### Release 7: Карточка рабочего / Worker card *
Так же необходимо реализовать подробную карточку рабочего, в которой пользователи приложения могут оставлять свои комментарии.
Подумай, как реализовать таблицы в Sequelize таким образом, чтобы комментариев к карточке было больше чем одио и от разных пользователей.

Функционал:
- Пользователь приложения может оставить коментарий
- Пользователь может отредактировать свой коментарий
- Пользователь не может редактировать не свои комментарии

![card mockup](readme-assets/6.PNG)  
*Рисунок 5*. Интерфейс менеджера.

## Заключение

Если вы не успели выполнить задание полностью, в любом случае залейте свой проект на GitHub и сформируйте Pull Request. Сделать это необходимо до 16.00.# hh_project