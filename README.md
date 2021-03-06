## Перевоз истории сообщений из групповых чатов Агента/ICQ в MyTeam

Перевозятся сообщения в виде 

```
user@corp.mail.ru 2020-04-01 12:04:20: Сообщение
```

Сохраняются реплаи на сообщения. Перевозятся файлы и картинки, которые были отправлены в чат. 

### Требования:
1) php7.2 и выше
2) [Composer](https://getcomposer.org/)

```bash
git clone https://github.com/tyaga/mrbot-history.git
cd mrbot-history
composer install
cp config.dist.php config.php
```

### Использование

1) Cоздать двух ботов - одного в myteam, второго в ICQ. При помощи скрипта ботом в ICQ будем читать сообщения из старого чата в ICQ, ботом в myteam будем писать сообщения в новый чат в myteam. Инструкция по созданию бота есть [тут для myteam](https://myteam.mail.ru/botapi/botTutorial.html) и [тут для icq](https://icq.com/botapi/botTutorial.html) - читать от п. **Регистрация бота**. 
2) Указать этим ботам (и в майтим и в ICQ) **/setjoingroups enable** и **/setprivacy Disable** . Для этого в @metabot (как указано в инструкции) сначала надо отправить сообщение **/setjoingroups** , затем отправляется имя бота, затем отправляется **enable**. Затем отправить **/setprivacy**, затем имя бота, затем **Disable** . 
3) В config.php заполнить ICQ_TOKEN и MTM_TOKEN . 
4) Эти два бота будут использоваться для перевозов всех ваших чатов. В описании ниже вместо @history_bot читать ник созданных вами ботов. 

### Перевоз истории

```bash
$ php -dmemory_limit=0 -dmax_execution_time=0 history.php 
```

### Действия в ICQ/Агент

#### Перевоз контактов чата ботом @chatsyncbot
Этап опциональный. Можно не делать, если перевозите в новый пустой чат, и не требуется синхронизировать контакты.

1) Найти в поиске @chatsyncbot, нажать у него "Начать" (делается 1 раз)
2) Добавить в группу @chatsyncbot
3) Сделать его админом
4) Написать в этот чат сообщение **/sync**
5) Написать в этот чат **/forward** (Бот должен ответить, что пересылка ВЫключена)

#### Получение ID группы в ICQ

1) Найти в поиске "Chat ID Bot", нажать у него "Начать" (делается 1 раз)
2) Добавить в группу "Chat ID Bot". Он сразу же из этого чата выйдет, и напишет в личку ID группы
3) Вставить этот ID в config.php в ICQ_GROUP_ID

#### Добавление в группу бота @history_bot

1) Найти в поиске @history_bot, нажать у него "Начать" (делается 1 раз)
2) Добавить в группу @history_bot 

### Действия в MyTeam

#### Получение ID группы в MyTeam

0) Сделать группу (или воспользоваться той вновь созданной группой, которую перевез syncbot)
1) Скопировать в инфо о группе ссылку, взять из нее ID 
```
https://u.internal.myteam.mail.ru/profile/**ТУТ_ID_ГРУППЫ**
```
2) Вставить этот ID в config.php в MTM_GROUP_ID

#### Добавление в группу бота @history_bot

1) Найти в поиске @history_bot, нажать у него "Начать" (делается 1 раз)
2) Добавить в группу @history_bot 

### После того, как в группе в myteam будет бот @history_bot и в группе в ICQ/Агенте будет @history_bot , а также в конфиге будут заполнены все ID и токены, можно запускать перевоз истории.
