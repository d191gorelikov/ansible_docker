#Автоматизированная настройка сервера с помощью Ansible и Docker

## Описание проекта по тестовому заданию Betting Software
Проект предоставляет инфраструктуру для автоматической настройки и управления сервером с помощью **Ansible** и **Docker**. Основные функции включают:

1. **Управление пользователями** (создание пользователей, добавление SSH-ключей, установка паролей и групп)
2. **Настройка SSH** (безопасная конфигурация, запрет root-доступа, логирование)
3. **Установка и настройка Nginx** 
4. **Развертывание статических файлов**.
5. **Установка Zsh и Oh My Zsh** для пользователей
6. **Обновление и установка утилит**
7. **Docker-контейнеризация** для развертывания и тестирования

---
Изображения из задания раздаются по адресу **http://51.250.71.54/images**

## Структура проекта

![image](https://github.com/user-attachments/assets/b4d8757f-33bf-4099-8b9b-6a2e50b9f31b)


# Основные роли Ansible

### 1. **apt** 
- Обновляет системные пакеты и устанавливает дополнительные утилиты.

### 2. **nginx**
- Устанавливает и настраивает Nginx с помощью Jinja-шаблона.

### 3. **ssh** 
- Запрещает root-доступ.
- Запрещает пустые пароли.
- Включает логирование в режиме **VERBOSE**.
- Отключает **X11Forwarding**.

### 4. **static**
- Копирует статические файлы в указанную директорию на сервере.

### 5. **usermanager**
- Создает пользователей.
- Добавляет SSH-ключи.
- Устанавливает пароли и группы.

### 6. **zsh**
- Устанавливает оболочку Zsh.
- Скачивает и устанавливает **Oh My Zsh** для пользователей с Zsh.

---


# Запуск проекта

### Шаг 1: Установка зависимостей
На хосте должны быть установлены:
- **Ansible**
- **Docker** и **docker-compose**

### Шаг 2: Запуск Docker-контейнера
Перейти в каталог docker и выполните команду:
```bash
docker-compose up -d
```

### Шаг 3: Запуск плейбука Ansible
Выполните команду для применения всех ролей:
```bash
ansible-playbook -i ansible/inventory/hosts ansible/playbook.yml
```


## Конфигурация пользователей
Список пользователей и их параметров задатся в переменных:

- `ansible/vars/users_admin.yml`
- `ansible/vars/users_developer.yml`
- `ansible/vars/users_tester.yml`
- и другие файлы в директории `vars`.

---

## Примечания
- **SSH-ключи**: Убедитесь, что файл `docker/authorized_keys` содержит корректные публичные SSH-ключи.  
- **Порты**: Nginx по умолчанию слушает порт `80`. При необходимости измените конфигурацию в файле `nginx_vhost.conf.j2`.
- **SSH подключение по паролю**: SSH подключение по паролю отключено. Для подлкючения к контейнеру под созданными пользователями нужно использовать связку ключей.
   Пример команды:
   ```bash
   ssh -i ~/.ssh/id_rsa -p 2222 admin@127.0.0.1
   ```
   
## Скриншоты
Скриншоты для подтверждения удачного запуска проекта и демонстрации работы проекта:
- Успешная сборка образа
   ![build](https://github.com/user-attachments/assets/4f6ec67c-e384-45f1-9fd3-b229acbab151)

- Успешный запуск Docker-composw   
![compose](https://github.com/user-attachments/assets/24ef5a44-da8f-487d-8a8a-30493d2ee240)

- Подключение к контейнеру с помощью закрытого ключа
  
![rsa](https://github.com/user-attachments/assets/bb1ef1f0-160c-4762-aed9-d4c118a2f064)

- Успешное выполение playbook`а и запуска всех ролей

![playbook](https://github.com/user-attachments/assets/321dc82d-3748-4457-80ab-20ad095e58ee)

   
   

