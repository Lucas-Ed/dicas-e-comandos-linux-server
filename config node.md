# Tutorial Node Umbrel, no orange Pi5 com somente o armazenamento da blockchain no SSD.


- Alterar data e hora, com fuso horário de são paulo.

```bash
sudo timedatectl set-timezone America/Sao_Paulo
```
- Alterar a senha de login no servidor :

```bash
passwd

```
- Instalar o pacote de acesso remoto por aplicativo no celular:

```bash
apt-get install openssh-server
```
pronto agora só usar o app ConnectBot para logar, para logar vc usa o host: root@seu-ip:22,
após isso coloque a senha e pronto.

- Configurar para não haver deconexão via SSH

você pode definir o valor da opção ClientAliveInterval no arquivo de configuração do servidor SSH (/etc/ssh/sshd_config) para um valor alto o suficiente para que a conexão não expire.

Para fazer isso, abra o arquivo /etc/ssh/sshd_config em um editor de texto com privilégios de superusuário usando o comando:

Isso é opicional:
```bash
ClientAliveInterval 300
```
Este exemplo define o intervalo para 300 segundos (5 minutos). Você pode aumentar esse valor se desejar.

Em seguida, adicione a seguinte linha para garantir que o SSH não desconecte automaticamente uma conexão inativa:

```bash
ClientAliveCountMax 0
```
- Salve o arquivo e saia do editor. Em seguida, reinicie o serviço SSH para que as alterações entrem em vigor:
```bash
sudo systemctl restart ssh
```
A partir de agora, a conexão SSH não será desconectada automaticamente por inatividade.
---
- instalar o "curl"

```bash
sudo apt install curl
```

- atualizar e limpar o sistema
```bash
sudo apt update

sudo apt full-upgrade

sudo apt autoremove

sudo apt autoclean
```

- No /root instale o umbrel normalmente

```bash
sudo curl -L https://umbrel.sh | bash
```

- Após a instalação o sistema dará o start no umbrel, é necessário para-lo:
  
```bash
sudo /root/umbrel/scripts/stop
```
---

- Verifique o disco, no meu caso é o /dev/sda1
```bash
sudo fdisk -l
```

- Montar o disco da blockchain,criar o local para a montagem
  
```bash
sudo mkdir /mnt/blockchain
```
- Antes de configurar a montagem desmonte o local padrão do sistema

```bash
sudo umount /media/*
```
- Agora monte na nova pasta escolhida
```bash
sudo mount /dev/sda1 /mnt/blockchain
```
## Configuração de montagem de unidade de disco ao ligar a máquina
Para saver a unidade:
```bash
sudo blkid
```
deve ser algo como /dev/sda1.

- Edite o arquivo /etc/fstab
O arquivo /etc/fstab é responsável por montar automaticamente dispositivos de armazenamento durante o processo de inicialização. Execute o seguinte comando para editar o arquivo:
```bash
sudo nano /etc/fstab
```
- Adicione a seguinte linha ao final do arquivo, substituindo /dev/sda1 pelo identificador do dispositivo que você descobriu no primeiro passo e /mnt/blockchain pelo diretório que você criou no segundo passo:

```bash
/dev/sda1 /mnt/blockchain auto defaults 0 0
```
- Dê a permissão necessária para o sistema montar a unidade:

```bash
sudo chmod 777 /mnt/blockchain
```
Salve e saia do editor de texto.

- Tente montar a unidade de disco manualmente
Se o identificador do dispositivo e o diretório de montagem estiverem corretos, tente montar a unidade de disco manualmente usando o seguinte comando:

```bash
sudo mount /dev/sda1 /mnt/blockchain
```
- Se a unidade de disco montar corretamente, verifique se os arquivos estão acessíveis no diretório de montagem. Se sim, desmonte a unidade de disco usando o seguinte comando:

```bash
sudo umount /dev/sda1
```
- Verifique o status do serviço de montagem automática
Verifique se o serviço de montagem automática está em execução usando o seguinte comando:

```bash
systemctl status systemd-fstab.service
```
- Se o serviço não estiver em execução, inicie-o usando o seguinte comando:

```bash
sudo systemctl enable systemd-fstab.service
```
Isso deve fazer com que o serviço seja executado automaticamente durante o processo de inicialização e monte a unidade de disco.

Reinicie o Orange Pi5
Agora, reinicie o seu Orange Pi5 para que as alterações feitas no arquivo /etc/fstab sejam aplicadas:

```bash
sudo reboot
```

- Se retornar falha, ao ativar a unidade: o arquivo de unidade systemd-fstab.service não existe.
- Abra um editor de texto como o nano usando o seguinte comando:

```bash
sudo nano /etc/systemd/system/systemd-fstab.service
```
- Copia e cole o seguinte conteúdo no arquivo:

```bash
[Unit]
[Unit]
Description=Automount local filesystems

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/bin/mount -a
ExecStop=/bin/umount -a
TimeoutSec=5s
StandardOutput=syslog
StandardError=syslog

[Install]
WantedBy=multi-user.target


```

- Salve o arquivo e saia do editor de texto.

Atualize o daemon do systemd para que ele reconheça o novo arquivo de unidade usando o seguinte comando:

```bash
sudo systemctl daemon-reload
```
- Habilite o serviço para que ele seja iniciado automaticamente durante o processo de inicialização, usando o seguinte comando:

```bash
sudo systemctl enable systemd-fstab.service
```
- Verifique se o serviço foi ativado corretamente usando o seguinte comando:

```bash
sudo systemctl status systemd-fstab.service
```
Agora, o serviço de montagem automática deve estar funcionando corretamente e a unidade de disco deve ser montada automaticamente durante o processo de inicialização. Se você encontrar algum problema, verifique novamente as configurações do arquivo /etc/fstab e verifique se o identificador do dispositivo e o ponto de montagem estão corretos.

- Se retornar inativo, ative-o:

Se o comando sudo systemctl status systemd-fstab.service retornou "inativo", isso significa que o serviço de montagem automática não está em execução no momento. Você pode tentar iniciar o serviço manualmente com o comando sudo systemctl start systemd-fstab.service.

Se o serviço iniciar corretamente e o comando sudo systemctl status systemd-fstab.service mostrar que o serviço está ativo, tente reiniciar o sistema e verifique se a unidade de disco é montada automaticamente durante o processo de inicialização.

Se o serviço ainda não estiver funcionando corretamente, você pode verificar os logs do systemd para obter mais informações sobre o problema. Use o comando sudo journalctl -u systemd-fstab.service para ver os logs do serviço. Isso pode ajudá-lo a identificar e corrigir quaisquer problemas que estejam impedindo o serviço de funcionar corretamente.

## Agora configureo para manter montado o disco do SSD, quando houver inatividade e o equipamento estiver ligado

ara configurar o sistema Ubuntu Server para evitar que uma unidade de disco seja desmontada por inatividade, você pode usar o utilitário "systemd" para criar um serviço que mantenha a unidade montada.

Primeiro, verifique o nome do dispositivo da unidade de disco que deseja manter montada. Você pode usar o comando "lsblk" para listar os dispositivos de bloco e suas montagens atuais.

Crie um novo arquivo de serviço do systemd em "/etc/systemd/system/" com um nome descritivo, como "keep-mounts.service". Por exemplo, use o comando "sudo nano /etc/systemd/system/keep-mounts.service" para criar o arquivo e abri-lo para edição.

No arquivo do serviço, adicione o seguinte conteúdo:

1° crie o arquivo de serviço do systemd em "/etc/systemd/system/" com um nome descritivo, como "keep-mounts.service", usando o editor de texto "nano" é o seguinte:

```bash
sudo nano /etc/systemd/system/keep-mounts.service
```
2° Esse comando abrirá o editor de texto "nano" e criará um novo arquivo de serviço com o nome "keep-mounts.service" em "/etc/systemd/system/". Em seguida, você pode inserir o conteúdo do arquivo do serviço e salvar e fechar o arquivo usando as opções do editor de texto.

```bash
[Unit]
Description=Keep specified mounts from being unmounted by systemd
After=local-fs.target

[Service]
Type=oneshot
ExecStart=/bin/mount /dev/sda1
ExecStop=/bin/umount /dev/sda1
RemainAfterExit=yes

[Install]
WantedBy=local-fs.target


```
Substitua "/dev/sdXY" pelo nome do dispositivo da unidade de disco que deseja manter montada. Se a unidade estiver formatada em um sistema de arquivos específico, substitua "/bin/mount" pelo comando apropriado para montar o sistema de arquivos.

Salve e feche o arquivo.

Carregue o novo serviço do systemd com o comando "sudo systemctl daemon-reload".

Ative o serviço para que seja iniciado automaticamente durante a inicialização com o comando "sudo systemctl enable keep-mounts.service".

Inicie o serviço manualmente com o comando "sudo systemctl start keep-mounts.service".

Agora, o serviço "keep-mounts" manterá a unidade de disco especificada montada e evitará que ela seja desmontada por inatividade, mesmo se o sistema estiver ligado.

---

- Verificar se o acesso a esta pasta montada está acessível e visível.

```bash
ls /mnt/blockchain
```
- Caso indique "Permissão negada" faça...
```bash
sudo chmod -R 755 /mnt/blockchain/
```
- Repita

```bash
ls /mnt/blockchain
```

- Outra maneira é ver os arquivos dentro da pasta indo até ela:

```bash
cd /mnt/blockchain
```
e depois:

```bash
ls -l 
```
---
- Identificar o UUID próprio

Use o comando:
```bash
sudo blkid
```

- Irá aparecer algo assim:

```bash
algo assim parecido irá aparecer...
#/dev/sda1: UUID="865499E25499D4F1" BLOCK_SIZE="4096" TYPE="ext4"
PARTUUID="52113e7c-01"
#/dev/sda1: UUID="4a4e7c51-419c-40b9-8b02-be51d200a6b4" BLOCK_SIZE="1024" TYPE="ext2"
PARTUUID="6fba667b-01"
#/dev/sda5: UUID="ICq9u4-pC1H-dpRI-bwCA-QLfe-E691-QVUz2o" TYPE="LVM2_member" PARTUUID="6fba667b05"
#/dev/mapper/debian--vg-root: UUID="ff7661ff-8496-4d12-8bb6-dc3fe6f6c8a1" BLOCK_SIZE="4096"
TYPE="ext4"
#/dev/mapper/debian--vg-swap_1: UUID="3edb1fb1-10c8-4ee2-ac26-06e516c49921" TYPE="swap"

```
- copie o UUID, que no meu caso é:865499E25499D4F1

- Edite o "fstab" para colocar a linha do ID do disco da blockchain, use o comando:

```bash
sudo nano /etc/fstab
```
- Vc verá algo assim:

```bash
UUID=7adb1433-141c-458d-b47a-a35be4e5a1a3 / ext4 defaults,noatime,commit=600,errors=remount-ro 0 1
UUID=4B41-A486 /boot vfat defaults 0 2
tmpfs /tmp tmpfs defaults,nosuid 0 0
```
- Adicione o caminho para salvar a blockchain no seu ssd, use o UUID de seu disco SSD.
```bash
UUID=865499E25499D4F1 /mnt/blockchain  ext4 defaults,noatime,noauto,x-systemd.automount,errors=remount-ro 0 2
```
- Execute o umbrel

```bash
sudo /root/umbrel/scripts/start
```

- Abra no seu browser

```bash
http://orangepi5.local ou https://IP
```
- instalar a app "Bitcoin Node" do Umbrel e executar a app deixando ele começar a sincronizar, DEIXAR O BITCOIN COMEÇAR A MOSTRAR EVOLUÇÃO > 0,01%.

- Editar o arquivo exports.sh
```bash
nano /root/umbrel/app-data/bitcoin/exports.sh
```
- Altere o caminho do salvamento da blockchain para o diretório que será montado o seu SSD:
Substitua a linha:

```bash
export APP_BITCOIN_DATA_DIR="${EXPORTS_APP_DIR}/data/bitcoin"
```
por:

```bash
export APP_BITCOIN_DATA_DIR="/mnt/blockchain/data/bitcoin"
```
salve CTR-X e Y

- Dê o stop no umbrel
```bash
sudo /root/umbrel/scripts/stop
```
---

- Para poupar espaço no disco do sistema operativo, pode apagar as pastas da blockchain, execute os 3 comandos seguidamente.

```bash
rm -r ./umbrel/app-data/bitcoin/data/bitcoin/indexes/
rm -r ./umbrel/app-data/bitcoin/data/bitcoin/chainstate/
rm -r ./umbrel/app-data/bitcoin/data/bitcoin/blocks/
```

## Evitar, que o servidor umbrel não se desligue:

```bash
sudo nano /etc/systemd/logind.conf
```

Na linha onde está "HandleLidSwitch" , tire o "#" e substitua "suspend" por "ignore" -- p.ex.-

```bash
HandleLidSwitch=ignore

```
E em "IdleAction=ignore", tire o "#" para evitar que o orangepi não desligue por inatividade.

- Grave com Crt-x e Y, e a seguir vamos executar a ação de reiniciar o arquivo logind.conf, sem precisar de reiniciar o equipamento.

```bash
sudo systemctl restart systemd-logind.service
```
Pronto podemos iniciar o umbrel novamente:
```bash
sudo /root/umbrel/scripts/start
```
- Abra no seu browser

```bash
http://orangepi5.local ou https://IP
```
Abra o App bitcoin e aguarde a sincronização e o início do download dos blocos.

- Para checar o andamento da sincronização para download dos blocos abra o LOG do app, para acompanhar se está tudo ocorrendo perfeitamente.

```bash
sudo /root/umbrel/scripts/app compose bitcoin logs -f bitcoind
```
ou
```bash
sudo tail -f ~/root/umbrel/app-data/bitcoin/data/bitcoin/debug.log
```

Agora só aguardar baixar toda a blockchain, finalizamos este tutorial !



guardar do fstab
UUID=7adb1433-141c-458d-b47a-a35be4e5a1a3 / ext4 defaults,noatime,commit=600,errors=remount-ro 0 1
