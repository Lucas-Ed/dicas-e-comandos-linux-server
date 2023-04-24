Dicas e comandos Linux server

- Primeiro acesso via PuTTY:

```bash
root@seu-ip
```
- Fazer upgrade do sistema:

```bash
sudo apt-get update

// Ou
sudo upgrade
```

- Instalação o Docker e o Docker Compose:

```bash
sudo apt install docker docker-compose -y

```

- Alterar arquivo de tempo de inatividade:

```bash
// Acesse o arquivo:

sudo nano /etc/systemd/logind.conf

```

- Desligar o server:

```bash
// Desliga imeditamente:

sudo shutdown -h now

// Desliga o sistema em 10 minutos:

sudo shutdown -h +10

// Desliga o sistema em 10 segundos:

sudo shutdown -h 10

```
---
