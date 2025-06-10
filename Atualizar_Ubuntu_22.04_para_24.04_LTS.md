O Ubuntu 24.04 LTS (Noble Numbat) foi lançado a 25 de abril de 2024. Esta nova versão será suportada durante cinco anos, até junho de 2029. As atualizações incluem melhorias significativas em pacotes principais como o kernel Linux, systemd, Netplan, atualizações das ferramentas para melhor suporte ao desenvolvimento, medidas de segurança aprimoradas e otimizações de desempenho. Também possui um ambiente de ambiente de trabalho GNOME atualizado e outras aplicações padrão. Vamos ver como atualizar o Ubuntu 22.04 LTS para o Ubuntu 24.04 LTS usando a linha de comandos (CLI) numa sessão baseada em SSH.

## Passo 1 – Fazer Cópia de Segurança do Sistema
Fazer uma cópia de segurança dos seus dados antes de atualizar do Ubuntu 22.04 LTS para 24.04 LTS é essencial por duas razões. Primeiro, mesmo que o processo seja exaustivamente testado, podem surgir problemas inesperados durante a atualização. Se algo correr mal, uma cópia de segurança garante que pode recuperar ficheiros insubstituíveis, como bases de dados, código escrito em PHP/Perl/Python, documentos, fotos ou scripts. Segundo, atualizar para uma nova versão LTS pode introduzir alterações que causam incompatibilidade com alguns dos seus dados. Uma cópia de segurança permite restaurar e migrar os dados para um formato compatível com a nova versão do Ubuntu. Lembre-se de fazer uma cópia de segurança dos seus dados antes de atualizar para o Ubuntu. Não nos culpe se perder tudo!

Como Fazer Cópia de Segurança de Dados Importantes ou de Tudo?

Os fornecedores de nuvem geralmente oferecem opções de cópia de segurança, como tirar uma fotografia instantânea do seu servidor na nuvem (aqui está um guia para EC2 e Lightsail VM). Alternativamente, pode usar várias ferramentas de cópia de segurança, como rsnapshot, tarsnap, restic, kbackup, duplicity, bacula e Déjà Dup. Testar as suas cópias de segurança e verificar se podem ser restauradas é necessário, assim como descobrir quanto tempo leva a restaurar os dados.

## Passo 2 – Atualizar o Sistema
Execute o comando apt para atualizar todos os pacotes instalados no Ubuntu 22.04 LTS:

```bash
sudo apt update
```
```bash
sudo apt list --upgradable | more
```
```bash
sudo apt upgrade
```
Pode ver uma mensagem como esta enquanto atualiza o sistema 22.04 LTS:
```bash
Nova versão do kernel disponível<br>A versão atual do kernel em execução é x.xx.x, que não é a versão esperada x.xx.x2.  <br>O reinício do sistema para carregar o novo kernel não será tratado automaticamente, pelo que deverá considerar reiniciar. 
```

Portanto, reinicie a máquina Ubuntu Linux usando o comando reboot ou shutdown:

```bash
sudo reboot
```

## Passo 3 – Atualizar do 22.04 LTS para 24.04 LTS

Deve instalar o pacote ubuntu-release-upgrader-core:
```bash
sudo apt install ubuntu-release-upgrader-core
```

Certifique-se de que a linha Prompt em /etc/update-manager/release-upgrades está configurada para lts usando o comando grep ou cat:
```bash
grep 'lts' /etc/update-manager/release-upgrades
cat /etc/update-manager/release-upgrades
```

## Abrir a Porta TCP 1022 Usando o Comando ufw ou iptables

Para aqueles que utilizam sessões baseadas em SSH, abra uma porta SSH adicional usando o comando ufw, começando na porta 1022. Esta é a porta predefinida definida pelo procedimento de atualização como alternativa caso a porta SSH predefinida falhe durante as atualizações. A sintaxe para o comando ufw para abrir a porta alternativa TCP/1022 SSH com ufw é a seguinte:
```bash
sudo ufw allow 1022/tcp comment 'Abrir porta ssh TCP/1022 como medida de segurança para atualizações'
```
```bash
sudo ufw status
```

Aqui está um exemplo para iptables:
```bash
sudo /sbin/iptables -I INPUT -p tcp --dport 1022 -j ACCEPT
```

## Passo 4 – Atualizar do Ubuntu 22.04 LTS para a Versão Ubuntu 24.04 LTS

Finalmente, inicie a atualização da versão Ubuntu 22.04 para 24.04 LTS. Digite:
```bash
sudo do-release-upgrade -d
```

Verá uma mensagem de boas-vindas como a seguinte:

```bash
A verificar se há uma nova versão do Ubuntu

= Bem-vindo ao Ubuntu 24.04 LTS 'Noble Numbat' =

A equipa do Ubuntu tem o orgulho de anunciar o Ubuntu 24.04 LTS 'Noble Numbat'.

Para ver as novidades desta versão, visite:
  https://wiki.ubuntu.com/NobleNumbat/ReleaseNotes

O Ubuntu é uma distribuição Linux para o seu computador ou servidor, com uma instalação rápida e fácil, lançamentos regulares, uma seleção restrita de excelentes aplicações instaladas por predefinição, e quase qualquer outro software que possa imaginar disponível através da rede.

Esperamos que desfrute do Ubuntu.
...
...
Para se inscrever em futuros anúncios do Ubuntu, por favor, subscreva a lista de anúncios de baixo volume do Ubuntu em:

  http://lists.ubuntu.com/mailman/listinfo/ubuntu-announce

Continuar [sN]
```

Então, será informado sobre a porta SSH que já abriu:

```bash
A ler cache

A verificar o gestor de pacotes

Continuar a correr sob SSH?

Esta sessão parece estar a correr sob ssh. Não é recomendável realizar uma atualização através de ssh atualmente, porque em caso de falha é mais difícil recuperar.

Se continuar, será iniciado um daemon ssh adicional na porta
'1022'.
Deseja continuar?

Continuar [sN]
```

Finalmente, precisará de confirmar para iniciar o procedimento de atualização:

## Remover pacotes obsoletos?

Verá a seguinte mensagem:

```bash
Remover pacotes obsoletos?

27 pacotes serão removidos.

Continuar [sN]  Detalhes [d]
```

Reveja cuidadosamente estes pacotes e remova-os apenas se não precisar deles. Caso contrário, escolha a opção ‘N’.

## Atualização do Sistema Completa
O momento chegou. A atualização do sistema está completa. Tudo o que precisa fazer é confirmar com ‘S’ para reiniciar o sistema e torcer para que ele volte a funcionar:

## Passo 5 – Verificação
Use o comando lsb_release ou cat para verificar a versão do seu Ubuntu. Este comando consulta o /etc/os-release e fornece a informação da versão:
```bash
cat /etc/os-release
lsb_release -a
```

Deve mostrar:

```bash
PRETTY_NAME="Ubuntu 24.04 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```

Verifique a versão do kernel Linux da seguinte forma, usando o comando uname:

```bash
uname -mrs
```

Deve mostrar:

```bash
Linux 6.8.0-1008-aws x86_64
```

## Passo 6 – Ativar Repositórios/Mirrors de Terceiros

Após concluir a atualização do Ubuntu 22.04 LTS (ou 23.10) para 24.04 LTS, certifique-se de ativar mirrors e repositórios de terceiros; caso contrário, não receberá atualizações. Use o seguinte comando:

```bash
cd /etc/apt/sources.list.d
ls -l
```

Em seguida, execute o comando apt:
```bash
sudo apt update
sudo apt upgrade
```

Finalmente, limpe pacotes indesejados e não utilizados:

```bash
sudo apt autoremove --purge
```

Certifique-se de remover a regra de firewall iptables/ufw que foi adicionada anteriormente para abrir a porta SSH alternativa em TCP/1022. Por exemplo:

```bash
sudo ufw show added
# adicione a regra de exclusão antes da palavra-chave allow
sudo ufw delete allow 1022/tcp comment 'Abrir porta ssh tcp porta 1022 como opção de segurança para atualizações'
```
## Conclusão
Parabéns! Atualizou com sucesso o seu sistema Ubuntu de 22.04 LTS ou 23.10 para o mais recente 24.04 LTS usando a linha de comandos. Para detalhes mais aprofundados, consulte as notas de lançamento oficiais do Ubuntu 24.04 e leia as páginas de manual usando “man” ou “help”: