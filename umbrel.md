# Os comandos abaixo vão instalar todas as dependências necessárias:
```bash
sudo apt-get install fswatch jq rsync curl git
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
sudo apt-get install curl
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
- Para iniciar o serviço:
```bash
sudo ./scripts/start
```
- Acessar no browser:

http://seu-ip/start

ou

http://orangepi5.local


- Para verificar se o seu SSD está conectado ao seu servidor Ubuntu, você pode executar o seguinte comando no terminal:
```bash
sudo fdisk -l
```
## Formatar SSD pelo terminal

- Desmonte o dispositivo se ele já estiver montado com o comando "umount". Por exemplo, se o seu dispositivo for /dev/sda1, você pode usar o seguinte comando:
```bash
sudo umount /dev/sda1
```
- Use o comando "fdisk" para criar uma nova tabela de partições no SSD. Este comando irá excluir todas as partições existentes e criar uma nova tabela vazia. Certifique-se de selecionar o dispositivo correto antes de executar este comando. Por exemplo, se o seu dispositivo for /dev/sda, você pode usar o seguinte comando:
```bash
sudo fdisk /dev/sda

```

Pressione a letra "o" para criar uma nova tabela de partições do tipo DOS.

Pressione a letra "n" para criar uma nova partição.

Escolha o tipo de partição que você deseja criar. Por exemplo, se você quiser criar uma partição do tipo "Linux filesystem", escolha o tipo "83".

Defina o tamanho da partição e sua posição na tabela de partições.

Salve as alterações na tabela de partições usando o comando "w".

Crie um novo sistema de arquivos no SSD usando o comando "mkfs". Por exemplo, se você quiser criar um sistema de arquivos ext4, você pode usar o seguinte comando:

```bash
sudo mkfs.ext4 /dev/sda1

```
O seu SSD agora está formatado e pronto para uso. Você pode montá-lo em um diretório de sua escolha usando o comando "mount". Por exemplo, se você quiser montar o dispositivo em "/mnt/ssd", você pode usar o seguinte comando:

```bash
sudo mount /dev/sda1 /mnt/ssd
```
Lembre-se de substituir "/dev/sda1" pelo dispositivo correto que você quer formatar e montar, e "/mnt/ssd" pelo diretório de sua escolha para montar o dispositivo. Tenha cuidado ao executar esses comandos, pois eles podem excluir dados existentes em seu dispositivo.

Para sair use o w

- Parar o serviço Umbrel:
```bash
sudo umbrel stop
```

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