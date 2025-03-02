# Домашнее задание к занятию 11 «Teamcity» - Сергей Яремко

## Подготовка к выполнению

1. В Yandex Cloud создайте новый инстанс (4CPU4RAM) на основе образа `jetbrains/teamcity-server`.
2. Дождитесь запуска teamcity, выполните первоначальную настройку.
3. Создайте ещё один инстанс (2CPU4RAM) на основе образа `jetbrains/teamcity-agent`. Пропишите к нему переменную окружения `SERVER_URL: "http://<teamcity_url>:8111"`.
4. Авторизуйте агент.
5. Сделайте fork [репозитория](https://github.com/aragastmatb/example-teamcity).
6. Создайте VM (2CPU4RAM) и запустите [playbook](./infrastructure).

## Основная часть

1. Создайте новый проект в teamcity на основе fork.
2. Сделайте autodetect конфигурации.
3. Сохраните необходимые шаги, запустите первую сборку master.
4. Поменяйте условия сборки: если сборка по ветке `master`, то должен происходит `mvn clean deploy`, иначе `mvn clean test`.
5. Для deploy будет необходимо загрузить [settings.xml](./teamcity/settings.xml) в набор конфигураций maven у teamcity, предварительно записав туда креды для подключения к nexus.
6. В pom.xml необходимо поменять ссылки на репозиторий и nexus.
7. Запустите сборку по master, убедитесь, что всё прошло успешно и артефакт появился в nexus.
8. Мигрируйте `build configuration` в репозиторий.
9. Создайте отдельную ветку `feature/add_reply` в репозитории.
10. Напишите новый метод для класса Welcomer: метод должен возвращать произвольную реплику, содержащую слово `hunter`.
11. Дополните тест для нового метода на поиск слова `hunter` в новой реплике.
12. Сделайте push всех изменений в новую ветку репозитория.
13. Убедитесь, что сборка самостоятельно запустилась, тесты прошли успешно.
14. Внесите изменения из произвольной ветки `feature/add_reply` в `master` через `Merge`.
15. Убедитесь, что нет собранного артефакта в сборке по ветке `master`.
16. Настройте конфигурацию так, чтобы она собирала `.jar` в артефакты сборки.
17. Проведите повторную сборку мастера, убедитесь, что сбора прошла успешно и артефакты собраны.
18. Проверьте, что конфигурация в репозитории содержит все настройки конфигурации из teamcity.
19. В ответе пришлите ссылку на репозиторий.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---

##Ответ на Задание

Я нее стал использовать облако и создал виртуальные машины локально (ну как создал, они были) 

И так, подготовил сервер командугород:

```
docker run --name="teamcity-server" -d -p 8111:8111 jetbrains/teamcity-server
```

![](https://github.com/s-bessonniy/mnt-homeworks/blob/MNT-video/09-ci-05-teamcity/screenshots/VirtualBox_Ubuntu-50Gb_02_03_2025_10_34_03.png)

Далее подготовил агента командугород:

```
docker run -e SERVER_URL=http://192.168.10.4:8111 --name="teamcity-agent" -d jetbrains/teamcity-agent
```

![](https://github.com/s-bessonniy/mnt-homeworks/blob/MNT-video/09-ci-05-teamcity/screenshots/VirtualBox_%20Ubuntu_vm1_02_03_2025_10_48_50.png)

Проверяем, РАБОТАЕТ. Хоть что то у меня заработало с первого раза:

В огненной лисе вводим:

```
http//:192.168.10.4:8111
```

![](https://github.com/s-bessonniy/mnt-homeworks/blob/MNT-video/09-ci-05-teamcity/screenshots/VirtualBox_%20Ubuntu_vm1_02_03_2025_10_52_51.png)

Nexus будет стоят на виртуалке с УБУНТОЙ (почему тут все на Центосе?)) с айпишником 198.168.10.9. Наверное в комментах полно такого "У МЕНЯ НЕ РАБОТАЕТ":

В огненной лисе виртуалки за нумером 198.168.10.7 вводим:

```
http//:192.168.10.9:8081
```

![](https://github.com/s-bessonniy/mnt-homeworks/blob/MNT-video/09-ci-05-teamcity/screenshots/VirtualBox_%20Ubuntu_vm1_02_03_2025_12_40_38.png)

И так, создаем проект:

![](https://github.com/s-bessonniy/mnt-homeworks/blob/MNT-video/09-ci-05-teamcity/screenshots/VirtualBox_%20Ubuntu_vm1_02_03_2025_13_18_46.png)

Ошалеть, пока работает:

![](https://github.com/s-bessonniy/mnt-homeworks/blob/MNT-video/09-ci-05-teamcity/screenshots/VirtualBox_%20Ubuntu_vm1_02_03_2025_13_22_21.png)

Меняем условие сборки:

![](https://github.com/s-bessonniy/mnt-homeworks/blob/MNT-video/09-ci-05-teamcity/screenshots/VirtualBox_%20Ubuntu_vm1_02_03_2025_13_48_12.png)

![](https://github.com/s-bessonniy/mnt-homeworks/blob/MNT-video/09-ci-05-teamcity/screenshots/VirtualBox_%20Ubuntu_vm1_02_03_2025_14_03_11.png)

Nexus:

![](https://github.com/s-bessonniy/mnt-homeworks/blob/MNT-video/09-ci-05-teamcity/screenshots/VirtualBox_%20Ubuntu_vm1_02_03_2025_14_32_54.png)
