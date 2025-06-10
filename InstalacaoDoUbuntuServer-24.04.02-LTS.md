## 05_ Quinta Etapa: Instalação e Configuração do Ubuntu Server 22.04.x LTS

Link Oficial da Instalação do Ubuntu Server: https://ubuntu.com/server/docs/installation

```bash
01) *Try or Install Ubuntu Server
<Enter>

02) Use UP, DOWN and ENTER keys to select your language
    English (recomendado utilizar sempre a opção em Inglês)
<Enter>

03) Installer update available
    Version 25.04 of the installer is now available (24.08.1 is currently running)
<Continue without updating>

04) Keyboard configuration
    Layout: [Portuguese (Brazil)] (altere conforme a sua necessidade)
    Variant: [Portuguese (Brazil)] (altere conforme a sua necessidade)
<Done>

05) Choose type of install
    (X) Ubuntu Server (DEFAULT - Selecionado)
    ( ) Ubuntu Server (minimized)
    Additional options
      [ ] Search for third-party drivers
<Done>

06) Network connections
    enp0s3 eth - (o nome lógico da placa de rede muda de equipamento para equipamento)
    DHCPv4 172.16.1.XXX/24 (altere o Endereço IPv4 conforme a sua necessidade)
    #OBSERVAÇÃO IMPORTANTE: VERIFICAR O ENDEREÇO IPv4 QUE VOCÊ ESTÁ USANDO NA SUA REDE 
    #INTERNA PARA ADAPTAR NO SEU CENÁRIO.
<Done>

07) Configure proxy
    Proxy address: (Default)
<Done>

08) Configure Ubuntu archive mirror
    Mirror: http://archive.ubuntu.com/ubuntu
    #OBSERVAÇÃO IMPORTANTE: CASO QUEIRA TROCAR O MIRROR DO UBUNTU DO BRASIL PARA O
    #OFICIAL NO US, SUBSTITUA A URL DE: http://br.archive.ubuntu.com/ubuntu PARA A
    #URL: http://us.archive.ubuntu.com/ubuntu
<Done>

09) Guided storage configuration
    (X) Use an entire disk (Default)
      [VBOX_HARDISK-XXXX local disk 50.000G]
      () Set up this disk as an LVM group (Default)
          [] Encrypt the LVM group with LUKS (Default - No (Não))
<Done>

10) Storage configuration
    USED DEVICES
      ubuntu-lv  new, to be formatted as ext4, mounted at /  24G <Enter>
      Edit <Enter>
          Name: ubuntu-lv
          Size (max 47.996G): 47.996G
          Format: ext4
          Mount: /
      <Save>
<Done>
    Confirm destructive action
<Continue>

11) Profile setup
    #OBSERVAÇÃO: ALTERAR OS DADOS DO NOME DO SERVIDOR, USUÁRIO E SENHA PARA O SEU CENÁRIO.
    Your name: Seu Nome e Sobrenome <Tab>
    Your servers name: wsseunome <Tab>
    Pick a username: seu_usuário <Tab>
    Choose a passwords: sua_senha <Tab>
    Confirm your passwords: sua_senha <Tab>
<Done>

12) Upgrade to Ubuntu Pro
    (X) Skip Ubuntu Pro setup for now
<Continue>

13) SSH Setup
    [X] Install OpenSSH server: ON (Habilitar - pressione <Space> para selecionar)
    Import SSH identity: No (Default)
<Done>

14) Featured Server Snaps
<Done>

15) Install complete!
<Reboot Now>

16) Please remove the installation medium, then press ENTER:
<Enter>
```

## 06_ Sexta Etapa: Acessando o Ubuntu Server pela primeira vez via Terminal
```bash
#AGUARDAR A INICIALIZAÇÃO TOTAL DO UBUNTU SERVER, NO FINAL SERÁ GERADO VÁRIAS CHAVES 
#DE AUTENTICAÇÃO DO SSH SERVER, PRESSIONE <ENTER> PARA APARECER A TELA DE LOGIN.

01) Tela de Login do Ubuntu Server
    wsseunome login: seu_usuário <Enter> (altere para o seu usuário)
    Password: sua_senha <Enter> (altere para a sua senha)
```