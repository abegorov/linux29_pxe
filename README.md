# Настройка PXE сервера для автоматической установки

## Задание

1. Настроить загрузку по сети дистрибутива **Ubuntu 24.04**
2. Установка должна проходить из **HTTP**-репозитория.
3. Настроить автоматическую установку c помощью файла **user-data**.

## Запуск

Для запуска требуется **libvirt** (так как **PXE** загрузка невозможно через **EFI** прошивку в **VirtualBox 7.1.0** на **Linux**).

Необходимо скачать **VagrantBox** для **almalinux/9** версии **v9.4.20240805** и добавить его в **Vagrant** под именем **almalinux/9/v9.4.20240805**. Сделать это можно командами:

```shell
curl -OL https://app.vagrantup.com/almalinux/boxes/9/versions/9.4.20240805/providers/virtualbox/amd64/vagrant.box
vagrant box add vagrant.box --name "almalinux/9/v9.4.20240805"
rm vagrant.box
```

После этого нужно сделать **vagrant up**.

Протестировано в **OpenSUSE Tumbleweed**:

- **Vagrant 2.3.7**
- **VirtualBox 7.1.0_SUSE r164728**
- **Ansible 2.17.4**
- **Python 3.11.10**
- **Jinja2 3.1.4**

## Проверка
