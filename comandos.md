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

- Procure a linha que começa com "#IdleAction" e remova o "#" para descomentar a linha. Em seguida, altere o tempo:

```bash
IdleAction=ignore

```

- Pressione Ctrl + X, em seguida Y e Enter para salvar as alterações e sair; Reinicie o serviço do systemd-logind com o seguinte comando:

```bash
sudo systemctl restart systemd-logind.service


```
Pronto! Agora, o sistema não desligará mais automaticamente por inatividade.

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
