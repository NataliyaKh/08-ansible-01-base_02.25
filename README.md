# Домашнее задание к занятию 1 «Введение в Ansible». Наталия Ханова. 

## Подготовка к выполнению

1. Установите Ansible версии 2.10 или выше.
2. Создайте свой публичный репозиторий на GitHub с произвольным именем.
3. Скачайте [Playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.

## Основная часть

1. Попробуйте запустить playbook на окружении из `test.yml`, зафиксируйте значение, которое имеет факт `some_fact` для указанного хоста при выполнении playbook.
```
ansible-playbook -i inventory/test.yml site.yml
```
![some_fact](https://github.com/NataliyaKh/08-ansible-01-base_02.25/blob/main/screens/ansible-1-1.png)
```
some_fact = 12
```

2. Найдите файл с переменными (group_vars), в котором задаётся найденное в первом пункте значение, и поменяйте его на `all default fact`.

![all_default](https://github.com/NataliyaKh/08-ansible-01-base_02.25/blob/main/screens/ansible-1-2_change.png)

3. Воспользуйтесь подготовленным (используется `docker`) или создайте собственное окружение для проведения дальнейших испытаний.

![docker](https://github.com/NataliyaKh/08-ansible-01-base_02.25/blob/main/screens/ansible-1-3_surroundings.png)
Примечание: centos7 пришлось менять на контейнер с centos8, так как с первым была несовместима нужная версия python.
```
docker run -dit --name centos8 centos:8 /bin/bash

docker run -dit --name ubuntu ubuntu:20.04 /bin/bash
```

4. Проведите запуск playbook на окружении из `prod.yml`. Зафиксируйте полученные значения `some_fact` для каждого из `managed host`.
```
ansible-playbook -i inventory/prod.yml site.yml
```
![managed_host](https://github.com/NataliyaKh/08-ansible-01-base_02.25/blob/main/screens/ansible-1-4-ubuntu-centos.png)

5. Добавьте факты в `group_vars` каждой из групп хостов так, чтобы для `some_fact` получились значения: для `deb` — `deb default fact`, для `el` — `el default fact`.

6. Повторите запуск playbook на окружении `prod.yml`. Убедитесь, что выдаются корректные значения для всех хостов.

![new_facts](https://github.com/NataliyaKh/08-ansible-01-base_02.25/blob/main/screens/ansible-1-6-newsomefact.png)

7. При помощи `ansible-vault` зашифруйте факты в `group_vars/deb` и `group_vars/el` с паролем `netology`.
```
ansible-vault encrypt group_vars/deb/examp.yml

ansible-vault encrypt group_vars/el/examp.yml
```
![encrypt](https://github.com/NataliyaKh/08-ansible-01-base_02.25/blob/main/screens/ansible-1-7-encrypt.png)

8. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь в работоспособности.
```
ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
```
![ask-vault-pass](https://github.com/NataliyaKh/08-ansible-01-base_02.25/blob/main/screens/ansible-1-8-ask-vault-pass.png)

9. Посмотрите при помощи `ansible-doc` список плагинов для подключения. Выберите подходящий для работы на `control node`.
```
ansible-doc -t connection -l
```
![plugins](https://github.com/NataliyaKh/08-ansible-01-base_02.25/blob/main/screens/ansible-1-9-plugins.png)
```
ansible.builtin.local          execute on controller
```
10. В `prod.yml` добавьте новую группу хостов с именем  `local`, в ней разместите localhost с необходимым типом подключения.

![localhost](https://github.com/NataliyaKh/08-ansible-01-base_02.25/blob/main/screens/ansible-1-10-localhost.png)

11. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь, что факты `some_fact` для каждого из хостов определены из верных `group_vars`.

![hosts](https://github.com/NataliyaKh/08-ansible-01-base_02.25/blob/main/screens/ansible-1-11-3hosts.png)

12. Заполните `README.md` ответами на вопросы. Сделайте `git push` в ветку `master`. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым `playbook` и заполненным `README.md`.

13. Предоставьте скриншоты результатов запуска команд.
См. выше. 

## Необязательная часть

1. При помощи `ansible-vault` расшифруйте все зашифрованные файлы с переменными.
2. Зашифруйте отдельное значение `PaSSw0rd` для переменной `some_fact` паролем `netology`. Добавьте полученное значение в `group_vars/all/exmp.yml`.
3. Запустите `playbook`, убедитесь, что для нужных хостов применился новый `fact`.
4. Добавьте новую группу хостов `fedora`, самостоятельно придумайте для неё переменную. В качестве образа можно использовать [этот вариант](https://hub.docker.com/r/pycontribs/fedora).
5. Напишите скрипт на bash: автоматизируйте поднятие необходимых контейнеров, запуск ansible-playbook и остановку контейнеров.
6. Все изменения должны быть зафиксированы и отправлены в ваш личный репозиторий.

---

### Как оформить решение задания

Приложите ссылку на ваше решение в поле «Ссылка на решение» и нажмите «Отправить решение»
---
