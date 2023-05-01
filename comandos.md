# Dicas e comandos Linux server

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

- Ou Defina a política padrão para permitir somente conexões de saída com o seguinte comando:

```bash
sudo firewall-cmd --set-default-zone=drop

```

Este comando definirá a política padrão para "drop", o que significa que todas as conexões de entrada serão bloqueadas e todas as conexões de saída serão permitidas, a menos que sejam explicitamente permitidas.

- Permita o tráfego de entrada apenas para portas específicas, usando o seguinte comando:

```bash
sudo firewall-cmd --add-port=PORTA/tcp --permanent
```
Substitua "PORTA" pelo número da porta que você deseja permitir. Isso permitirá o tráfego de entrada para a porta especificada.

- Aplique as novas regras do firewall usando o seguinte comando:
  
```bash
sudo firewall-cmd --reload

```

- Verifique as configurações do FirewallD com o seguinte comando:

```bash
sudo firewall-cmd --list-all

```
Isso mostrará todas as regras definidas atualmente no FirewallD.

- Reinicie o FirewallD para aplicar as alterações com o seguinte comando:
  
```bash
sudo systemctl restart firewalld

```
- Permitir o tráfego de entrada, mas solicitar a sua permissão para cada conexão, você pode usar a zona "public" do FirewallD. Você pode seguir os seguintes passos:

- Defina a política padrão para a zona "public" com o seguinte comando:

```bash
sudo firewall-cmd --set-default-zone=public

```

- Crie uma regra para permitir conexões de entrada com o seguinte comando:

```bash
sudo firewall-cmd --add-rich-rule='rule family="ipv4" source address="0.0.0.0/0" port port=0 protocol="tcp" accept'

```

Esta regra permitirá todas as conexões de entrada na porta 0 (todas as portas) para qualquer endereço IP.

- Reinicie o FirewallD para aplicar as alterações com o seguinte comando:

```bash
sudo systemctl restart firewalld

```
Agora, o FirewallD permitirá conexões de entrada, mas solicitará sua permissão para cada conexão. Quando uma conexão de entrada for detectada, o FirewallD exibirá uma mensagem no terminal e pedirá que você confirme se deseja permitir a conexão.

Cada vez que uma nova conexão de entrada for detectada, o FirewallD solicitará novamente sua permissão. Se você deseja permitir a conexão permanentemente, poderá adicionar uma regra permanente ao FirewallD usando o comando --add-rich-rule e a opção --permanent.

## Aplicar a autenticação de dois fatores (2FA) no Linux server.

- Instale o pacote de autenticação de dois fatores google-authenticator usando o gerenciador de pacotes da distribuição Linux, Debian/Ubuntu, execute o seguinte comando:

```bash
sudo apt install libpam-google-authenticator
```

- Vá para o arquivo:

```bash
sudo nano /etc/pam.d/common-auth
```

- No arquivo, abaixo de (the "primary" block), coloque:
  
```bash
auth required pam_google_authenticator.so
```

Execute o comando google-authenticator para configurar o Google Authenticator no seu servidor. Ele irá gerar um código QR e uma chave secreta que você deve armazenar com segurança. Escaneie o código QR usando um aplicativo de autenticação de dois fatores em seu smartphone, como o Google Authenticator ou o Authy, e salve a chave secreta.

- Vá para o arquivo:

```bash
sudo nano /etc/pam.d/sshd
```
- No arquivo, abaixo de # Read enviroment variables from /etc/enviroment and, coloque:
  
```bash
auth required pam_google_authenticator.so
```
- Vá para o arquivo:

```bash
sudo nano /etc/ssh/sshd_config
```

Altere o ChallengeResponseAuthenticaion de no pra yes.

Salve e feche o arquivo.

Reinicie o serviço sshd para aplicar as alterações:

```bash
sudo systemctl restart sshd
```

Teste a autenticação de dois fatores: Tente fazer login no servidor via SSH usando suas credenciais de login regulares. Depois de inserir sua senha, você será solicitado a inserir um código de autenticação gerado pelo aplicativo Google Authenticator. Insira o código e você deverá ter acesso ao servidor.

Lembre-se de armazenar a chave secreta em um local seguro, pois ela é necessária para configurar o Google Authenticator em um novo dispositivo. Além disso, é importante criar uma chave de backup em caso de perda ou roubo do dispositivo principal.

- [ Video de referência](https://bit.ly/3NcHl3g) 


## Navegação entre diretórios


- Ver o diretório atual:

```bash
pwd

```
- Navegar entre outros diretórios: 

```bash

cd /home/usuario/musicas

```

- Voltar ao diretório anterior:

```bash

cd ..

```
- Abrir um arquivo
```bash
nano nome-do-arquivo

```

## Login e senha do servidor

- Alterar a senha de login no servidor :

```bash
passwd

```
após isso digite a nova senha 2 vezes.

- Para alterar o nome de usuário do usuário atual para o novo nome de usuário:

```bash
usermod -l novo_nome_usuario nome_usuario_atual

```

- Mover o diretório home do usuário atual para o novo diretório home do usuário com o novo nome de usuário:

```bash
usermod -d /home/novo_nome_usuario -m novo_nome_usuario

```

- Outro método, se o nome de usuário atual é "usuario1" e o novo nome de usuário desejado é "usuario2", você pode usar os seguintes comandos:

```bash

sudo usermod -l usuario2 usuario1
sudo usermod -d /home/usuario2 -m usuario2

```

## Verificação de portas abertas no sistema.

- Para exibir as portas TCP e UDP abertas, abra o terminal e digite o seguinte comando:

```bash
sudo netstat -tulpn

```

Este comando exibirá uma lista de todas as conexões TCP e UDP abertas no sistema, juntamente com os processos que estão ouvindo ou usando essas portas.

A opção -t exibe as conexões TCP, a opção -u exibe as conexões UDP, a opção -l exibe as conexões de escuta e a opção -p exibe o nome e o ID do processo associado a cada conexão.

Para exibir apenas as portas abertas em um formato mais legível, você pode filtrar a saída do netstat usando o comando grep. Por exemplo, para exibir apenas as portas TCP abertas, você pode usar o seguinte comando:

```bash

sudo netstat -tulpn | grep LISTEN

```
Este comando exibirá apenas as conexões TCP de escuta, que indicam as portas TCP abertas no sistema.

## Verificar temperatura do hardware.

- Atualize o sistema:

```bash
sudo apt-get update

```

-  instale o pacote lm-sensors digitando o seguinte comando:

```bash
sudo apt-get install lm-sensors

```

- execute o comando abaixo para detectar sensores de temperatura:

```bash
sudo sensors-detect

```

- Durante a execução do comando acima, você será solicitado a responder algumas perguntas. Aceite as opções padrão (pressione Enter) para todas as perguntas.

Por fim, execute o comando abaixo para exibir as informações de temperatura:

```bash
sensors
```

## Alterar a linguagem do Ubuntu Server para Português do Brasil.

- instale o pacote "language-pack-pt" digitando o seguinte comando e pressionando Enter:
  
```bash
sudo apt-get install language-pack-pt

```
- Após a instalação, execute o comando abaixo para configurar a linguagem padrão do sistema para Português do Brasil:

```bash
sudo update-locale LANG=pt_BR.UTF-8

```

- Reinicie o sistema digitando o seguinte comando e pressionando Enter:

```bash
sudo reboot
```

- Após reiniciar, o idioma do sistema será alterado para Português do Brasil. Verifique se a mudança foi aplicada digitando o seguinte comando no terminal:

```bash
locale
```

Isso exibirá a configuração de idioma atual. Certifique-se de reiniciar o sistema para que as alterações entrem em vigor.


- Mostrar usuário:

```bash
whoami
```

- Restartar ssh:

```bash
service ssh restart
```
## Alterar a cor das letras do terminal

- Abra o arquivo .bashrc no seu editor de texto preferido, como o nano:

```bash
nano ~/.bashrc
```

- Role para baixo até encontrar a 2° linha que começa com PS1=. Essa linha define o prompt do seu terminal. O prompt padrão no Ubuntu Server geralmente se parece com isso:

```bash
PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
```

- Adicione um código de cor ANSI ao prompt. Por exemplo, para mudar a cor para verde, adicione o seguinte código antes do caractere $:

```bash
\[\033[32m\]$
```
- A linha deve ficar assim:

```bash
PS1='${debian_chroot:+($debian_chroot)}\[\033[32m\]\u@\h:\w\$\[\033[0m\] '

```

Salve e saia do arquivo. No nano, você pode fazer isso pressionando Ctrl+X, Y e Enter.

- Recarregue o arquivo .bashrc para aplicar as alterações:

```bash
source ~/.bashrc
```
Agora, o prompt do seu terminal deve estar verde. Você pode mudar a cor para outras cores, substituindo 32 pelo código de cor ANSI correspondente. Aqui está uma lista de alguns códigos de cor comuns:

Vermelho: 31
Verde: 32
Amarelo: 33
Azul: 34
Roxo: 35
Ciano: 36

## Memória RAM

- Ver comsumo:
```bash
free -h
```

- Exibir a porcentagem de memória RAM utilizada no sistema:

```bash
free | grep Mem | awk '{print $3/$2 * 100.0}'

```

## Alterar data e hora, com fuso horário de são paulo.

```bash
sudo timedatectl set-timezone America/Sao_Paulo
```

## Mover pasta de um diretório para outro.

- Digite o comando "cd" seguido do diretório que contém a pasta que você deseja mover. Por exemplo, se a pasta que você deseja mover estiver em "/home/usuario/pasta", digite:
```bash
cd /home/usuario/pasta
```
- Digite o comando "mv" seguido do nome da pasta que você deseja mover e o diretório de destino para onde deseja mover a pasta. Por exemplo, se você deseja mover a pasta "docs" para o diretório "/home/usuario/backup", digite:

```bash
mv docs /home/usuario/backup
```
Isso irá mover a pasta "docs" para o diretório "/home/usuario/backup". Certifique-se de digitar o caminho completo do diretório de destino para onde deseja mover a pasta. Se o diretório de destino não existir, o comando "mv" irá renomear a pasta para o novo nome especificado.