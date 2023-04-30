# Os comandos abaixo vão instalar todas as dependências necessárias:

Antes de instalar o umbrel é importante frizar que vc deve ter o SSD já formatada como um "pendrive"

## Montar SSD como pasta
No ubuntu server, no orange pi é necessário fazer algumas configurações para instalar o umbrell no SSD.

- Desmonte o diretório padrão do "pendrive"(SSD) conectado:
  
```bash
sudo umount /media/*
```
- Crie uma pasta no diretório /root
  
```bash
mkdir nome-da-sua-pasta
```
- Veja o diretório que está localizado com o comando:

```bash
sudo fdisk -l
```
no meu caso é o /dev/sda

- Monte	o SSD na pasta
```bash
sudo mount /dev/sda /nome-da-sua-pasta
```
Acesse sua pasta no /root usando o comando cd.

pronto, agora essa pasta é o seu "pendrive"(SSD), e vc pode continuar com a istalação do umbrell, dentro dessa pasta.

###  Instalações

```bash
sudo apt-get install curl
```

```bash
sudo apt install  ca-certificates  curl  gnupg  lsb-release
```

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```bash
echo  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu   $(lsb_release -cs) stable" 
```

```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y
sudo systemctl status docker
// iniciar caso não esteje
sudo systemctl start docker
sudo apt install docker-compose
```

### Download Umbrel (dentro de uma pasta vazia)
```bash
mkdir umbrel
```
- Para usar o comando ls installar o pacote:
```bash
sudo apt-get update
sudo apt-get install coreutils
```
```bash
ls
```

```bash
cd umbrel/
```

```bash
sudo apt-get update
sudo apt-get install snapd
sudo snap install yq
sudo apt-get update
sudo apt-get install gettext
sudo ln -s /usr/bin/envsubst /usr/local/bin/envsubst
```

- Agora basta baixar e descompactar o Umbrel. Caso você decida instalar o umbrel em um HD externo, lembre-se de criar a pasta dela no local adequado, visto que vamos precisar cerca de 500GB de espaço livre!
```bash
curl -L https://github.com/getumbrel/umbrel/archive/v0.5.3.tar.gz | tar -xz --strip-components=1
```
- Instalação em outros diretórios de outros SSD'S(+ de 1):
```bash
curl -L https://github.com/getumbrel/umbrel/archive/v0.5.3.tar.gz | tar -xz --strip-components=1 -C /media/user/SSDexterno/umbrel
```
ou
```bash
curl -L https://umbrel.sh | bash -s -- --install-path path/to/external/drive
```

- Para iniciar o serviço:

```bash
sudo ./scripts/start
```
- Acessar no browser:

http://seu-ip/start

ou

http://orangepi5.local

---

- Para verificar se o seu SSD está conectado ao seu servidor Ubuntu, você pode executar o seguinte comando no terminal:
```bash
sudo fdisk -l
```
---
## Formatar SSD pelo terminal

- Abra um terminal e verifique se o SSD está listado usando o seguinte comando:
  
```bash
sudo fdisk -l

```

- Isso mostrará uma lista de dispositivos de armazenamento conectados ao sistema, incluindo o SSD.

- Passo 3: Use o comando "umount" para desmontar o SSD antes de formatá-lo. Isso garantirá que nenhum processo esteja usando o dispositivo no momento da formatação. O comando "umount" é usado da seguinte maneira:

```bash
sudo umount /dev/<device>
```

- Substitua "<device>" pelo caminho correto do dispositivo listado no passo 2. Por exemplo, se o SSD estiver listado como "/dev/sda1", o comando seria:

```bash
sudo umount /dev/sda1
```

- Passo 4: Use o comando "mkfs" para formatar o SSD. Para armazenar a blockchain do nó Bitcoin, recomenda-se usar o sistema de arquivos "ext4". O comando para formatar o SSD em "ext4" é:

```bash
sudo mkfs.ext4 /dev/<device>
```
- Substitua "<device>" pelo caminho correto do dispositivo listado no passo 2. Por exemplo, se o SSD estiver listado como "/dev/sdb1", o comando seria:

```bash
sudo mkfs.ext4 /dev/sda1
```

- Passo 5: Monte o SSD formatado em um diretório de sua escolha. Crie um diretório para montar o SSD usando o seguinte comando:

```bash
sudo mkdir /mnt/<directory>
```

- Substitua "<directory>" pelo nome do diretório que você deseja criar. Por exemplo, se você quiser criar um diretório chamado "bitcoin-blockchain", o comando seria:

```bash
sudo mkdir /mnt/blockchain
```

- Passo 6: Monte o SSD no diretório que você acabou de criar usando o seguinte comando:

```bash
sudo mount /dev/<device> /mnt/<directory>
```

- Substitua "<device>" pelo caminho correto do dispositivo listado no passo 2 e "<directory>" pelo diretório que você criou no passo 5. Por exemplo, se o SSD estiver listado como "/dev/sdb1" e você criou um diretório chamado "bitcoin-blockchain", o comando seria:

```bash
sudo mount /dev/sda1 /mnt/blockchain
```
- Passo 7: Verifique se o SSD está montado corretamente usando o comando "df -h". Isso mostrará uma lista de todos os sistemas de arquivos montados no sistema. Certifique-se de que o SSD esteja listado e que o tamanho do espaço livre seja correto.

- Agora o SSD externo está formatado e pronto para ser usado para armazenar a blockchain do nó Bitcoin. Certifique-se de que o nó Bitcoin esteja configurado para usar o novo diretório de armazenamento.

- Configurar para ser montado na inicialização do ubuntu:

- Passo 1: Conecte o SSD externo ao seu Orange Pi 5 e execute o comando abaixo para verificar se ele foi detectado corretamente pelo sistema:

```bash
sudo fdisk -l
```
- Passo 2: Anote o caminho do dispositivo do SSD externo. Por exemplo, o caminho pode ser /dev/sda1.

- Passo 3: Crie o diretório no qual você deseja que o SSD seja montado. Por exemplo, se você deseja montar o SSD em /mnt/bitcoin-blockchain, execute o comando abaixo:

```bash
sudo mkdir /mnt/blockchain
```
- Passo 4: Abra o arquivo /etc/fstab com um editor de texto, como o nano, usando o seguinte comando:

```bash
sudo nano /etc/fstab
```
- Passo 5: Adicione uma nova linha ao arquivo /etc/fstab, usando o caminho do dispositivo e o diretório que você criou no Passo 3, no seguinte formato:

```bash
/dev/<device> /mnt/<directory> ext4 defaults 0 2
```
- Substitua <device> pelo caminho correto do dispositivo e <directory> pelo diretório que você criou no Passo 3. Por exemplo, se o SSD estiver listado como /dev/sda1 e você criou um diretório chamado bitcoin-blockchain, a linha seria:

```bash
/dev/sda1 /mnt/blockchain ext4 defaults 0 2
```

- Passo 6: Salve e feche o arquivo /etc/fstab.

- Passo 7: Execute o seguinte comando para montar o SSD externo no diretório que você criou:

```bash
sudo mount -a
```
Agora, o SSD externo será montado automaticamente no diretório que você especificou sempre que o sistema operacional for iniciado.

---

- Modificar salvamento da blockchain da raíz para o SSD:

Dentro da pasta umbrel vá até o diretório /app-data/bitcoin, entre no arquivo exports.sh, modifique a linha:
```bash
export app_bitcoin_data_dir="${exports_app_dir}/data/bitcoin"
```
Para:
```bash
export app_bitcoin_data_dir="/dev/ssd1/data/bitcoin"
```
- Reinicie o umbrel com ocomando:
```bash
sudo systemctl restart umbrel
```

- Algumas questões no forúm.
https://bit.ly/40LTHCz

- Artigo:

https://bit.ly/3NqwXoM

---

## Permissões

Primeiro seu usuário 
Segundo número grupo dele
Terceiro outros.


0 = sem permissão;
1 = somente execução;
2 = somente gravar;
3 = gravar e executar;
4 = somente ler;
5 = ler e executar;
6 = ler e gravar;
7 = ler, executar e gravar.

Chmod 777 você vai liberar permissão pra todos usuários resumindo
---
## Outros comandos
- Parar o serviço Umbrel:
```bash
sudo umbrel stop
```

- Descobrir o UUID dos discos conectados:

```bash
blkid
```
Você deverá ver: /dev/sda1: UUID="28ff522c-46c5-486e-ad59-aaba3ee8d879"

- Instalar o umbrell direto no HD/SSD:

```bash
https://umbrel.sh | bash -s -- --install-path path/to/external/drive
```

- Verificar espaço ocupado do SSD:
```bash
df
```

- Mostrar as permissões dos diretórios e arquivos:
```bash
Ls -l
//é pra m
```