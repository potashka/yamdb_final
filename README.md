# yamdb_final
Реализована регистрация с кодом подтверждения и дальнейшая авторизация с использованием JWT токена, при отправке запроса к API.

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).

В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха.

Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.

Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.

## bage
https://github.com/potashka/yamdb_final/actions/workflows/main.yml/badge.svg

## Требования
- Python (3.7+)

### Пакеты:
Django, Django REST Framework, PostgreSQL, Simple-JWT, Gunicorn, Docker, nginx, Yandex Cloud(Ubuntu 18.04), Git

## Установка
Клонировать репозиторий и перейти в него в командной строке:
```bash
git clone git@github.com:potashka/api_yamdb.gitGIT
cd api_yamdb
```
Установить и активировать виртуальное окружение:
```bash
python3 -m venv venv
source venv/bin/activate
```
Установить зависимости из файла requirements.txt:
```bash
python3 -m pip install --upgrade pip
pip install -r requirements.txt
```
Выполнить миграции:
```bash
python3 manage.py migrate
```
Добавление тестовых данных (импорт из фикстур)
```
python manage.py csvfullfillment
```
Запустить проект:
```bash
python3 manage.py runserver
```

## Примеры запросов
### Регистрация 
Отправляет на почту код подтверждения для получения токена.
```
POST /api/v1/auth/signup/
Content-Type: application/json

{
  "email": "user@example.com",
  "username": "user"
}
```
### Получение токена
```
POST /api/v1/auth/token/
Content-Type: application/json

{
  "username": "user",
  "confirmation_code": "blwe40-caa91ba6bc59d8a5bc3bded3b4c56972"
}
```

## Документация
Полный список эндпоинтов можно посмотреть запустив сайт и перейдя по ссылке `/redoc/`

## Докер/ создание образа и запуск контейнера:

Собрать образы Docker можно следующей командой:

docker-compose --project-directory ./infra up -d --build
или из директории infra docker-compose up -d --build # собрать образ 
docker-compose exec web python manage.py migrate # миграции
docker-compose exec web python manage.py createsuperuser # создание суперпользователя
docker-compose exec web python manage.py collectstatic --no-input # загрузка статики

Теперь проект доступен по адресу http://localhost/


После выполенения всех команд, при необходимости, можно загрузить фикстуры командой:

docker-compose --project-directory ./infra exec web python manage.py loaddata fixtures.json

шаблон файла .env:
  DB_ENGINE=django.db.backends.postgresql
  DB_NAME=postgres
  POSTGRES_USER=
  POSTGRES_PASSWORD=
  DB_HOST=db
  DB_PORT=5432
  SECRET_KEY = 

## Автоматизация развертывания серверного ПО

Для автоматизации развертывания ПО на боевых серверах используется среда виртуализации Docker, а также Docker-compose - инструмент для запуска многоконтейнерных приложений. Docker позволяет «упаковать» приложение со всем его окружением и зависимостями в контейнер, который может быть перенесён на любую Linux -систему, а также предоставляет среду по управлению контейнерами. Таким образом, для разворачивания серверного ПО достаточно чтобы на сервере с ОС семейства Linux были установлены среда Docker и инструмент Docker-compose.

В файле «docker-compose.yml» описываются запускаемые контейнеры: веб-приложения, СУБД PostgreSQL и сервера Nginx.
docker-compose.yamlbдолжен разворачивать контейнер web, используя образ, который вы создали на Docker Hub.

В файле в файле .github/workflows/yamdb_workflow.yml четыре задачи (job):

- проверка кода на соответствие стандарту PEP8 (с помощью пакета flake8) и запуск pytest из репозитория yamdb_final;
- сборка и доставка докер-образа для контейнера web на Docker Hub;
- автоматический деплой проекта на боевой сервер;
- отправка уведомления в Telegram о том, что процесс деплоя успешно завершился.

## Рзавернутый проект

http://84.252.143.225/admin/login/?next=/admin/
login: admin password: admin

## Авторы
- [Айдрус](https://github.com/zamaev), [Алексей](https://github.com/potashka), [Марсель](https://github.com/honour4life)

  
