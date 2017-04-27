DSS (Dvizh Shop Skeleton)
===============================

Скелетон основан на Yii2 модулях Dvizh и сторонних разработчиков, предназначен для быстрой сборки MVP (минимально жизнеспособный продукт) Интернет-магазина. Данное решение навязывает свою модульно-аспектную архитектуру приложения, направленную на

* Быструю разработку первой версии приложения за счет использования готовых конфигурируемых CRUD модулей, сервисов, поведений и виджетов;
* Долгосрочное развитие и поддержку без необходимости полного рефакторинга или переписывания с нуля до того момента, пока проект не "выстрелит".

Целевая аудитория - опытные программисты.

Скелетон имеет высокий порог входа по сравнению с CMS. Для того, чтобы собрать магазин на готовых модулях Dvizh, нужно знать:

* PHP (синтаксис ООП обязателен)
* HTML
* Yii2 фреймворк
* Git

Скелетон использует в качестве базы Yii2, большинство терминов берутся из документации этого фреймворка:

* Скелетон - базовый каркас приложения, зависящий от некоторых модулей и содержаший в себе аспекты с информацией о том, как модули связываются в рамках обслуживаемой предметной области;
* Модуль - максимально изолированный от других модулей набор классов;
* CRUD - контроллеры и вью файлы, формирующие и сохраняющие данные через модели;
* Сервис - экземпляр объекта (как правило, синглтон), доступный глобально в системе через yii::$app->serviceName и предоставляющий API для работы с какими-дибо данными;
* Событие - некоторое событие в модуле, которое потенциально может "заинтересовать" другие модули и само приложение. Подписчики события могут повлиять на логику выполнения программы, в которой это событие произошло. Аналог хуков в WordPress;
* Модель - AR модель, содержашая геттеры\сеттеры сущности и примитивное API для работы с данными сущности;
* Виджет - переносимый HTML кода с CSS и JS обработчиками. Аналог компонента в Битриксе;
* Аспект - коллбек, знающий о всех модулях в системе и реагирующий на события в этих  модулях. Как правило, реализуется посредством поведений Yii2. С точки зрения архитектуры приложения скелетона Dvizh, аспект находится на пересечении модулей;

Модули
===============================
Используемые в скелетоне важные модули:

* [dvizh/yii2-shop](https://github.com/dvizh/yii2-shop) - CRUD для управления товарами, модификациями, ценами, категориями, производителями, значениями фильтров;
* [dvizh/yii2-filter](https://github.com/dvizh/yii2-filter) - CRUD для фильтров на сайте, набор виджетов для быстрого применения фильтов на фронте с помощью Ajax;
* [dvizh/yii2-field](https://github.com/dvizh/yii2-field) - CRUD управления кастомными полями любой AR сущности;
* [dvizh/yii2-gallery](https://github.com/dvizh/yii2-gallery) - виджет загрузки картинок, виджет вывода картинок в виде галереи;
* [dvizh/yii2-seo](https://github.com/dvizh/yii2-seo) - виджет позволяет прикреплять к моделям важные для SEO поля;
* [dvizh/yii2-order](https://github.com/dvizh/yii2-order) - CRUD заказов, набор виджетов для создания заказа и с аналитическими отчетами о заказах за период;
* [dvizh/yii2-cart](https://github.com/dvizh/yii2-cart) - сервис и набор виджетов для быстрой разработки функционала корзины на сайте. В корзину можно положить любую модель, имплементируюущую нужный интерфейс;
* [dvizh/yii2-promocode](https://github.com/dvizh/yii2-promocode) - CRUD для управления скидками на сайте, сервис работы со скидками;
* [dvizh/yii2-certificate](https://github.com/dvizh/yii2-certificate) - CRUD для управления подарочными сертификатами на сайте, сервис работы со скидками;
* [dektrium/yii2-user](https://github.com/dektrium/yii2-user) - CRUD управления пользователями сайта;
* [dektrium/yii2-rbac](https://github.com/dektrium/yii2-rbac) - RBAC для сайта.


Установка
===============================

Существует два способа установки модулей:
1) Через клонирование репозитория Git'ом в кастомную папку Yii2 проекта и подключение модуля в секции autoload composer.json файла (+выполните composer update). Не забудьте подключить модуль в секции bootstrap, extensions и modules конфига (по аналогии с модулем от dektrium в данном скелетоне)
2) Через Composer:

```
composer create-project --prefer-dist --stability=dev dvizh/shop-skeleton
```

Выполните git init. Вы создали свою собственную ветку DSS, осталось собрать с помощью виджетов фронт нужного заказчику сайта.

Выполните следующую команду в корне проекта:

```
php init
```

Обновите Composer:

```
composer update
```

Настройте подключение к БД в следующих файлах:    

*/commom/config/main-local.php
*/console/config/main-local.php

Выполнение миграций модулей:

```
php yii migrate/up --migrationPath=@yii/rbac/migrations
php yii migrate/up --migrationPath=common/modules/dektrium/yii2-user/migrations
php yii migrate/up --migrationPath=migrations
php yii migrate/up --migrationPath=vendor/dvizh/yii2-shop/src/migrations
php yii migrate/up --migrationPath=vendor/dvizh/yii2-order/src/migrations
php yii migrate/up --migrationPath=vendor/dvizh/yii2-cart/src/migrations
php yii migrate/up --migrationPath=vendor/dvizh/yii2-filter/src/migrations
php yii migrate/up --migrationPath=vendor/dvizh/yii2-field/src/migrations
php yii migrate/up --migrationPath=vendor/dvizh/yii2-seo/src/migrations
php yii migrate/up --migrationPath=vendor/dvizh/yii2-gallery/src/migrations
php yii migrate/up --migrationPath=vendor/dvizh/yii2-promocode/src/migrations
php yii migrate/up --migrationPath=vendor/dvizh/yii2-certificate/src/migrations
```

Если у вас стоит Apache, на этом всё, .htaccess файлы идут в составе скелетона, все запросы пользователя будут направления на /frontend/web.
Если Apache нет, то придется еще настроить точки входа для фронтенда (frontend/web) и бекенда (backend/web).

Панель администрирования по умолчанию:

/backend/web/

administrator:10111988

Кастомизация
===============================
Рассмотрим основные способы кастомизации модулей без вынесения их в отдельную Git ветку.

1) Через создание дополнительного поля в модуле dvizh/yii2-field. Модули предлагают только CRUD бекенда, фронт пишется своими силами и в нем можно использовать API модуля yii2-field для поиска записей по кастомным полями и для вывода их содержимого.
2) Через viewPath. Когда нужно добавить дополнительную колонку в GridView и т.д.
3) Через controllerMap модуля. Когда нужно добавить поле в модель и модуль dvizh/yii2-field уже не подходит.
4) Через контрибьюцию. Если вы создали виджет, который может быть потенциально полезен сообществу или создали полезный метод в существующем API - добро пожаловать в контрибьюторы. Сделайте форк и пулл-реквест, тоталитарное руководство рассмотрит. Если это изменение несет в себе полезный для всех функционал, а не закрытие частного возражения, правка будет принята в центральный репозиторий.
5) Через отдельную ветку. Самый нежелательный вариант, сигнализирующий о том, что проект перерос MVP. Вы можете формнуть модуль и вести независимую разработку.

Использование
===============================
Использовать компоненты модулей нужно согласно документации этих модулей (пока что все в файле README).
