## Instalar samba

```bash
apt install samba -y
```

Fazer uma cópia do arquivo original do samba
```bash
cp /etc/samba/smb.conf /etc/samba/smb_original.bkp
```

```bash
vim /etc/samba/smb.conf
```

/etc/samba/smb.conf

```bash
[sky]

comment = diretorio arquivos executaveis de sky
writeable = yes
browseable = yes
path = /sky/executaveis/
create mask = 0777
directory mask = 0777
force create mode = 0777
force directory mode = 0777
guest ok = yes
read only = no
veto files = /*.mp3/*.mp4/*.doc/*.docx
delete veto files = no
vfs objects = recycle
recycle:keeptree = true
recycle:repository = /sky/lixeira
```

```bash
[backup]

comment = diretorio arquivos de backups de bancos de dados de sistemas sky
writeable = yes
browseable = yes
path = /sky/backup/
guest ok = yes
read only = yes
```

Reiniciar o serviço do samba
```bash
systemctl restart smbd
```

Criar um usuário padrão com acesso ao samba.

```bash
useradd skysmb -s /bin/false -c "usuario acesso samba" -M && \
smbpasswd -a skysmb
```