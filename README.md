# Network File System (NFS)

NFS permite *compartilhar* um diretório localizado em um computador em rede com outros computadores ou dispositivos nessa rede. O computador onde o diretório está localizado é chamado de servidor e os computadores ou dispositivos conectados a esse servidor são chamados de clientes.

## Instalação

No terminal, digite o seguinte comando para instalar o NFS Server:

```
sudo apt install nfs-kernel-server
```
Para iniciar o NFS Server, você pode executar o seguinte comando em um terminal:

```
sudo systemctl start nfs-kernel-server.service
```
## Configuração

Você pode configurar os diretórios a serem exportados adicionando-os ao arquivo `/etc/exports`. Por exemplo:

```
/srv     hostname1(ro,sync,subtree_check) hostname2(rw,async,no_subtree_check,no_root_squash)
/home    *.hostname.com(rw,sync,no_subtree_check) *(rw,async,no_subtree_check,no_root_squash)
```
Certifique-se de que todos os pontos de montagem personalizados que você está adicionando foram criados (/srv e /home já existem):

Aplique a nova configuração via:

```
sudo exportfs -a
```
Você pode substituir * por um dos formatos de nome de host. Faça a declaração do nome do host o mais específica possível para que sistemas indesejados não possam acessar a montagem NFS.

## Configuração do NFS Client

Para ativar o suporte NFS em um sistema cliente, digite o seguinte comando no terminal:

```
sudo apt install nfs-common
```
Use o comando `mount` para montar um diretório NFS compartilhado de outra máquina, digitando uma linha de comando semelhante à seguinte em um terminal:

```
sudo mkdir /opt/exemplo
sudo mount exemplo.hostname.com:/srv /opt/exemplo
```
>**Aviso**:  
O diretório do ponto de montagem `/opt/exemplo` deve existir. Não deve haver arquivos ou subdiretórios no diretório `/opt/exemplo`, caso contrário eles ficarão inacessíveis até que o sistema de arquivos NFS seja desmontado.

Uma maneira alternativa de montar um compartilhamento NFS de outra máquina é adicionar uma linha ao arquivo `/etc/fstab`. A linha deve indicar o nome do host do servidor NFS, o diretório no servidor que está sendo exportado e o diretório na máquina local onde o compartilhamento NFS será montado.

A sintaxe da linha no arquivo `/etc/fstab` é a seguinte:

```
exemplo.hostname.com:/srv /opt/exemplo nfs rsize=8192,wsize=8192,timeo=14,intr
```
Use o comando `mount -a` para montar todos os sistemas de arquivos mencionados no arquivo `/etc/fstab`, digite o seguinte comando no terminal:

```
sudo mount -a
```