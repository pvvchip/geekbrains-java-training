# Предварительное ТЗ на разработку итогового командного проекта

Проект будет представлять собой интерактивный сборник задач для обучающихся программированию на Java. Задача предоставляется пользователю в виде заготовки исходного кода класса, который он должен дописать непосредственно в текстовом редакторе веб-интерфейса. Проверка правильности решения осуществляется на стороне сервера с помощью юнит-теста, заранее подготовленного автором задачи. Результат выполнения теста возвращается пользователю. 

### Предполагаемая функциональность готового проекта
1. Возможность выбирать и решать имеющиеся в системе задачи. Онлайн-редактирование Java-кода как минимум с подсветкой синтаксиса. 
2. Возможность добавлять в систему новые задачи через веб-интерфейс.
3. Управление пользователями. Возможность зарегистрировать новую учетную запись, войти под ней. Учетная запись должна хранить информацию об уже решенных данным пользователем задачах.
4. Система рейтингов и оценок. Задачи должны быть ранжированы по степени сложности. Учетная запись пользователя должна хранить информацию о количестве рейтинговых баллов, заработанных данным пользователем. Рейтинговые баллы начисляются пользователю за решение задач в количестве, зависящем от их сложности. Рейтинговые баллы используются для ранжирования пользователей системы, а также могут быть потрачены пользователем на открытие "правильного ответа" в случае, если он не может решить некоторую задачу, но хочет увидеть решение.
5. Возможность для пользователей обсуждать решение задачи в комментариях к этой задаче. Открытие страницы комментариев к задаче должно уменьшать количество рейтинговых баллов, получаемых пользователем за решение.

### Технический подход к написанию проекта
1. Используется платформа Spring Boot.
2. Для реализации веб-интерфейса системы используется Thymeleaf + bootstrap для верстки. JS на стороне клиента стремимся использовать по минимуму и только в случае необходимости. Например, он может понадобиться для реализации web-редактора Java-кода.
3. Сущность "задача" на стороне сервера включает в себя (помимо метаданных - описания, уровня сложности и т.д.):
    1. Исходный код "заготовки" решения.
    2. Класс с юнит-тестами, которые призваны проверить корректность решения, предложенного пользователем.
    3. Исходный код, являющийся "правильным ответом".
4. Решение задачи, предлагаемое пользователем представляет собой исходный код Java-класса.
5. Сервер компилирует этот исходный код с помощью Compiler API, получает class-файл, который динамически загружается в JVM сервера. При этом используется техника создания "песочницы" для защиты сервера от потенциально вредоносного кода.
6. Сервер находит класс с юнит-тестами, соответствующий данной задаче, подгружает его в "песочницу" и запускает тесты с помощью библиотеки JUnit.
7. Результат тестирования отображается пользователю и если все тесты завершились успешно, то данная задача отмечается как решенная текущим пользователем, ему начисляются рейтинговые баллы и т.д.

### Стек технологий
1. Основа: Spring Boot;
2. Frontend: Thymeleaf + интеграция с Security;
3. База данных: PostgreSQL;
4. Компиляция: Compiler API;
5. Фреймворк для тестирования: JUnit;
6. Миграция БД: Flyway?
7. Безопасность: Spring Security;
8. Для работы с БД: Spring Data JPA;

### Как примерно должен выглядеть веб-интерфейс системы
1. Должно присутствовать меню + рабочая область. При выборе того или иного пункта меню, рабочая область отображает соответствующее содержимое. Меню присутствует на экране все время, изменяется только содержимое рабочей области. Состав меню должен зависеть от того, зашел ли пользователь на сайт под своей УЗ или работает с сайтом анонимно. Для анонимных пользователей часть пунктов меню будет недоступно. Например, анонимный пользователь не может являться автором новой задачи.
2. Примерный состав пунктов меню сайта и что они должны отображать в рабочей области:
    1. "Задачи". В рабочей области отображается список имеющихся задач. Должна быть возможность сортировать этот список например по сложности задачи или фильтровать по автору. Выбор задачи из списка заставляет рабочую область отобразить страницу выбранной задачи.
    2. Страница задачи должна содержать несколько разделов - "постановка задачи" (описание, предоставленное автором), "решение" (отображается веб-редактор кода с заготовкой решения и кнопка "Проверить"), "обсуждение" (отображаются комментарии пользователей, а также форма добавления нового комментария), "правильный ответ" (отображается веб-редактор кода с "правильным ответом", предоставленным автором задачи).
    3. "Рейтинг". В рабочей области отображается список пользователей системы с учетом количества имеющихся у них рейтинговых баллов
    4. "Добавить задачу". В рабочей области отображается мастер, с помощью которого автор задачи может добавить ее в систему. Первая страница - описание, вторая - заготовка решения, третья - юнит-тесты, четвертая - правильный ответ. Реализуется с помощью Spring WebFlow.
    5. "Профиль пользователя". В рабочей области отображаются данные, хранящиеся в УЗ текущего пользователя, с возможностью редактирования.

### Блоки задач
0. Простой frontend;
1. Компиляция класса из текстового фрагмента;
2. Прогон класса через тесты и получения результатов этих тестов;
3. Нужна основная страница с задачами, с возможностями:
    1. Отображения описания задачи;
    2. Рабочая область с редактором кода (ACE Editor);
    3. Область с отображением результатов тестирования;
    4. Область отображения комментариев;
    5. Возможность просмотра правильного ответа;
4. Регистрация и аутентификация пользователей (возможно +OAuth2);
5. Работа с профилем пользователя;
6. Админ-панель:
    1. Добавление/редактирование задач;
    2. Просмотр информации о пользователях;
7. Интеграция песочниц для выполнения пользовательского кода, и защита сервера от нежелательного кода пользователей;
8. Комментарии пользователей к задачам (на первом этапе на отдельных страницах);
9. Система рейтингов и страница с отображением этих рейтингов;
10. Страницу со справочной информацией по Java, добавить возможность пользователям писать 
что-то вроде коротких статей по темам (при определенном уровне рейтинга). Такие статьи должны
модерироваться админами. За статьи может начисляться дополнительный рейтинг;
11. Почтовые рассылки с оповещением о появлении новых задач/материалов/статей;
12. Может быть форум для обсуждение всего что касается Java;

### Вопросы
1. Надо ли требовать определенное количество баллов для открытия задач, или все задач доступны сразу