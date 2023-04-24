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

## Boas práticas com segurança

### Firewall

- Instalação e configuração, verifique se o firewall(Verifica o status), não está instalado ou em execução usando o seguinte comando::

```bash
sudo systemctl status firewalld


```
- Caso não estiver instalado, instale o firewall firewalld usando o seguinte comando:

```bash
sudo apt-get update
sudo apt-get install firewalld


```

- Inicie o serviço do firewall usando o seguinte comando:

```bash
sudo systemctl start firewalld

```

- Configure as regras do firewall para permitir ou bloquear o tráfego de entrada e saída. Você pode usar o seguinte comando para permitir tráfego HTTP e HTTPS:

```bash
sudo firewall-cmd --add-service=http --add-service=https --permanent


```
- Aplique as novas regras do firewall usando o seguinte comando:
  
```bash
sudo firewall-cmd --reload


```

## Aplicar a autenticação de dois fatores (2FA) no Linux server.

- Instale o pacote de autenticação de dois fatores google-authenticator usando o gerenciador de pacotes da distribuição Linux, Debian/Ubuntu, execute o seguinte comando:

```bash
sudo apt-get install libpam-google-authenticator


```

Execute o comando google-authenticator para configurar o Google Authenticator no seu servidor. Ele irá gerar um código QR e uma chave secreta que você deve armazenar com segurança. Escaneie o código QR usando um aplicativo de autenticação de dois fatores em seu smartphone, como o Google Authenticator ou o Authy, e salve a chave secreta.

Edite o arquivo /etc/pam.d/sshd para incluir a autenticação de dois fatores. Abra o arquivo /etc/pam.d/sshd em um editor de texto e adicione a seguinte linha no início do arquivo:

```bash
auth required pam_google_authenticator.so

```

Salve e feche o arquivo.

Reinicie o serviço sshd para aplicar as alterações:

```bash
sudo systemctl restart sshd


```

Teste a autenticação de dois fatores: Tente fazer login no servidor via SSH usando suas credenciais de login regulares. Depois de inserir sua senha, você será solicitado a inserir um código de autenticação gerado pelo aplicativo Google Authenticator. Insira o código e você deverá ter acesso ao servidor.

Lembre-se de armazenar a chave secreta em um local seguro, pois ela é necessária para configurar o Google Authenticator em um novo dispositivo. Além disso, é importante criar uma chave de backup em caso de perda ou roubo do dispositivo principal.

- criar uma chave de backup em caso de perda ou roubo do dispositivo principal 


