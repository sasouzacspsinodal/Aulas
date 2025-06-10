Definir a senha de super usuario root

```bash
passwd root
```

Atualizar o sistema:

```bash
apt-get upgrade
```

Atualizar indice de pacotes

```bash
apt-get update
```
Acertar fuso de horas no ubuntu 17 em diante

```bash
dpkg-reconfigure tzdata
```

Instalar unzip no sistema:

```bash
apt-get install unzip
```

Se necessário ajustar o IP após a instalação.

```bash
vim /etc/netplan/*.yaml
```

Exemplo quando configurado uma unica placa de rede:

```bash
network:
ethernets:
eno1:
addresses:
- 10.10.1.254/24
gateway4: 10.10.1.254
nameservers:
addresses:
- 8.8.8.8
- 8.8.4.4
version: 2
```
 
Após qualquer alteração em arquivo de configuração de rede aplique o comando:

```bash
netplan apply
```

Se necessário ajustar o hostname.

```bash
vim /etc/cloud/cloud.cfg
```
altere o parâmetro da linha preserve_hostname: de false para o valor true salve o arquivo.

Edite o arquivo para o nome desejado.

```bash
vim /etc/hostname
```