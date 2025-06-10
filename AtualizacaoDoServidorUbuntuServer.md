## 01_ Atualizando as Listas sources.list do Apt ou Apt-Get no Ubuntu Server

#Update é utilizado para baixar informações de pacotes de todas as fontes configuradas.
#opção do comando apt: update (Resynchronize the package index files from their sources)

```bash
sudo apt update
```

## 02_ Verificando todos os pacotes a serem atualizados no Ubuntu Server

#List é utilizado para listar todos os software que serão atualizados no sistema.
#opção do comando apt: list (list is used to display a list of packages), --upgradable (shows a 
#list of packages that can be upgraded using the apt package manager)

```bash
sudo apt list --upgradable
```

## 03_ Atualizando todos os software no Ubuntu Server

#Upgrade é utilizado para instalar atualizações disponíveis de todos os pacotes atualmente 
#instalados no sistema a partir das fontes configuradas via sources.list
#opção do comando apt: upgrade (Install the newest versions of all packages currently installed
#on the system from the sources enumerated in /etc/apt/sources.list.)

```bash
sudo apt upgrade
```

## 04_ Forçando uma atualização completa de todos os software e dependências no Ubuntu Server

#Dist-Upgrade além de executar a função de atualização, também lida de forma inteligente 
#com as novas dependências das novas versões de pacotes
#opção do comando apt: dist-upgrade (dist-upgrade in addition to performing the function of upgrade, 
#also intelligently handles changing dependencies with new versions of packages)

```bash
sudo apt dist-upgrade
```

## 05_ Forçando uma atualização e remoção de software desnecessários no Ubuntu Server

#Full-Upgrade executa a função de atualização, mas removerá os pacotes atualmente 
#instalados se isso for necessário para atualizar o sistema como um todo
#opção do comando apt: full-upgrade (Perform the function of upgrade but may also remove installed
#packages if that is required in order to resolve a package conflict)

```bash
sudo apt full-upgrade
```

## 06_ Removendo todos os pacotes desnecessários no Ubuntu Server

#Autoremove é utilizado para remover pacotes que foram instalados automaticamente para 
#satisfazer dependências de outros pacotes e agora não são mais necessários, pois as 
#dependências foram alteradas ou os pacotes que precisavam deles foram removidos nesse 
#meio tempo.
#opção do comando apt: autoremove (Autoremove is used to remove packages that were automatically
#installed to satisfy dependencies)

```bash
sudo apt autoremove
```

## 07_ Fazendo a limpeza dos repositórios locais e pacotes desnecessários no Ubuntu Server

#Autoclean como Clean, o autoclean limpa o repositório local de arquivos de pacotes 
#recuperados. A diferença é que ele remove apenas arquivos de pacotes que não podem 
#mais ser baixados e são inúteis.
#opção do comando apt: autoclean (Like clean, autoclean clears out the local repository of 
#retrieved package files)

```bash
sudo apt autoclean
```

## 08_ Limpando o cache local do sources.list no Ubuntu Server

#Clean limpa o repositório local de arquivos de pacotes recuperados
#opção do comando apt: clean (clean clears out the local repository of retrieved package files)

```bash
sudo apt clean
```

## 09_ Verificando todas as versões de software atualizados no Ubuntu Server

#List é utilizado para listar todos os software que serão atualizados no sistema.
#opção do comando apt: list (list is used to display a list of packages), --installed (shows
#a list of packages names as well as options to list installed)

```bash
sudo apt list --installed
```

## 10_ Verificando os Logs de atualização de software no Ubuntu Server

#Verificando o Log de instalação e atualização de pacotes no Ubuntu Server
#opção do comando cat: -n (number line)

```bash
sudo cat -n /var/log/apt/history.log
```

#Verificando o Log de finalização da atualização de pacotes no Ubuntu Server
#opção do comando cat: -n (number line)
```bash
sudo cat -n /var/log/apt/term.log
```

## 11_ Reiniciando o sistema operacional do Ubuntu Server

#Reiniciar o servidor para testar as atualizações

```bash
sudo reboot
```