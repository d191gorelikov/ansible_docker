FROM ubuntu:24.04

RUN apt-get update && apt-get install -y \
    openssh-server sudo && \
    mkdir -p /run/sshd  

# Создание пользователя ansible без пароля, вход только со связкой ключей для безопасности + добавление пользователя ansible к группе sudo 
RUN useradd -m -s /bin/bash ansible && \
    mkdir -p /home/ansible/.ssh && \
    chown -R ansible:ansible /home/ansible/.ssh && \
    chmod 700 /home/ansible/.ssh && \
    echo "ansible ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Добавляем публичный ключ в контейнер для пользака  ansible
COPY authorized_keys /home/ansible/.ssh/authorized_keys
RUN chown ansible:ansible /home/ansible/.ssh/authorized_keys && \
    chmod 600 /home/ansible/.ssh/authorized_keys

# Открываем порты 22 и 80 (хотя не обязательно тк мы можем их открыть и через docker-compose)
EXPOSE 22 80

CMD ["/usr/sbin/sshd", "-D"]

