# Tutorial Node Umbrel No orange Pi5 com somente o armazenamento da blockchain no SSD

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
---

- Verifiqueo disco, no meu caso é o /dev/sda1
```bash
sudo fdisk -l
```

- MONTAR O DISCO DA BLOCKCHAIN,criar o local para a montagem
  
```bash
sudo mkdir /mnt/blockchain
```
-Antes de configurar a montagem desmonte o local padrão do sistema

```bash
sudo umount /media/*
```
- Agora monte na nova pasta escolhida
```bash
sudo mount /dev/sda1 /mnt/blockchain
```
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
#/dev/sda1: UUID="e60dadac-09bf-4ce5-a632-ec925c4b252a" BLOCK_SIZE="4096" TYPE="ext4"
PARTUUID="52113e7c-01"
#/dev/sda1: UUID="4a4e7c51-419c-40b9-8b02-be51d200a6b4" BLOCK_SIZE="1024" TYPE="ext2"
PARTUUID="6fba667b-01"
#/dev/sda5: UUID="ICq9u4-pC1H-dpRI-bwCA-QLfe-E691-QVUz2o" TYPE="LVM2_member" PARTUUID="6fba667b05"
#/dev/mapper/debian--vg-root: UUID="ff7661ff-8496-4d12-8bb6-dc3fe6f6c8a1" BLOCK_SIZE="4096"
TYPE="ext4"
#/dev/mapper/debian--vg-swap_1: UUID="3edb1fb1-10c8-4ee2-ac26-06e516c49921" TYPE="swap"

```
- copie o UUID, que no meu caso é:e60dadac-09bf-4ce5-a632-ec925c4b252a

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
UUID=e60dadac-09bf-4ce5-a632-ec925c4b252a /mnt/blockchain  ext4 errors=remount-ro 0 2
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
- 
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
export APP_BITCOIN_DATA_DIR="/mnt/blockchain/bitcoin"
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
- Evitar, que o servidor umbrel não se desligue:


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