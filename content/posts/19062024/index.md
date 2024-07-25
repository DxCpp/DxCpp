---
title: "Как перенаправить WWW в Nginx"
date: 2024-06-19
description: "Перенаправление трафика с не-WWW на WWW или наоборот является обычным требованием для обеспечения постоянного доступа к вашему веб-сайту, улучшения SEO и улучшения пользовательского опыта. Такая настройка гарантирует, что все посетители будут направлены на одну версию вашего сайта, что помогает индексировать поисковые системы и устраняет проблемы с дублированием контента."
tags: ["NGINX", "Linux", "WWWW","SEO"]
type: post
weight: 20
showTableOfContents: true
---
## Понимание перенаправлений URL-адресов NGINX
Прежде чем изучать реализацию перенаправлений с WWW на не-WWW (и наоборот) в NGINX, важно понять основные концепции перенаправлений URL. Понимание этих основ обеспечивает эффективное и дружественное к SEO управление веб-сайтом.

### Понимание перенаправлений URL-адресов NGINX: основные типы
- **301 перенаправления:**
    - Цель: указать постоянное изменение URL-адреса.
    - Вариант использования: идеальный вариант, когда веб-страница навсегда переехала в новое место.
    - Влияние SEO: Переносит рейтинги SEO со старого URL на новый, сохраняя авторитет поисковых систем.
- **302 перенаправления:**
    - Цель: Обозначить временное изменение URL-адреса.
    - Вариант использования: полезно во время обслуживания сайта или временной смены контента, сигнализируя о будущем возврате к исходному URL-адресу.
    - Влияние на SEO: сообщает поисковым системам о необходимости сохранить исходный URL-адрес в индексе, поскольку изменение не является постоянным.
- **303 перенаправления:**
    - Цель: Управление отправкой форм путем предотвращения повторной отправки данных при обновлении страницы.
    - Вариант использования: в основном применяется в ситуациях, связанных с подтверждением отправки форм.
    - Пользовательский опыт: улучшает пользовательский опыт, предотвращая дублирование отправки форм и потенциальные ошибки данных.
## Перенаправление не-WWW на WWW URL в Nginx
### Перенаправление с помощью блоков сервера Nginx
Блоки сервера Nginx эффективны для управления перенаправлениями. Вот как можно перенаправить с URL-адреса Non-WWW на URL-адрес WWW.

**Пример конфигурации:**
```
server {
    listen 80;
    server_name example.com;
    return 301 $scheme://www.example.com$request_uri;
}

server {
    listen 80;
    server_name www.example.com;
    # Host your website content here
}
```
Эта конфигурация перенаправляет запросы с example.com (не-WWW) на www.example.com (WWW). $scheme и $request_uri сохраняют протокол и путь.

### Директивы перенаправления Nginx
Директивы перенаправления предлагают простой способ реализации перенаправлений.

**Пример директивы:**
```
server {
    listen 80;
    server_name example.com;
    return 301 $scheme://www.example.com$request_uri;
}
```
Эта директива перенаправляет example.com (без WWW) на www.example.com (WWW), сохраняя исходный путь URL и протокол.

## Перенаправление WWW на URL без WWW в Nginx
### Использование серверных блоков Nginx для WWW и не-WWW
Вот как настроить серверный блок, перенаправляющий с URL-адресов WWW на URL-адреса без WWW.

**Пример конфигурации:**
```
server {
    listen 80;
    server_name www.example.com;
    return 301 $scheme://example.com$request_uri;
}

server {
    listen 80;
    server_name example.com;
    # Host your website content here
}
```
Эта настройка перенаправляет **www.example.com** на **example.com**, используя `$scheme` и `$request_uri` для плавного перехода.

## Реализация перенаправлений с помощью правил перезаписи Nginx
Правила перезаписи в Nginx — это эффективный способ обработки манипуляций с URL-адресами.

**Пример правила переписывания:**
```
server {
    listen 80;
    server_name www.example.com;
    rewrite ^(.*)$ $scheme://example.com$1 permanent;
}
```
Это правило изменяет **www.example.com** на **example.com**. Шаблон `^(.*)$` захватывает все URL-адреса, добавляя их к `example.com ($1)`, и отмечает перенаправление как постоянное.

## Заключительные мысли
Настроив NGINX для перенаправления не-WWW на WWW или наоборот, вы обеспечиваете постоянный доступ к своему веб-сайту и улучшаете SEO. Регулярно проверяйте конфигурацию NGINX, чтобы убедиться, что она работает так, как ожидается, и вносите коррективы при необходимости. Реализация этих перенаправлений помогает поддерживать единое присутствие в сети и улучшает пользовательский опыт, направляя весь трафик на одну версию вашего сайта.