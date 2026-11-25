# Roteiro de Aula -- Instalação e Configuração do Zabbix 8.0 LTS

**Disciplina:** Arquitetura e Configuração de Servidores\
**Sistema operacional:** Ubuntu Server 26.04 LTS\
**Ferramenta:** Zabbix 8.0 LTS\
**Virtualização:** Hyper-V\
**Banco de dados:** MySQL\
**Servidor Web:** Apache

> **Objetivo:** instalar o Zabbix em um servidor Ubuntu, configurar o
> banco de dados e o Apache, acessar a interface web e realizar os
> primeiros testes de monitoramento.

## 1. Conhecendo o Zabbix

O Zabbix é uma ferramenta de monitoramento de infraestrutura de TI.
Permite acompanhar servidores, computadores, equipamentos de rede,
serviços e aplicações.

Entre as informações que podem ser monitoradas estão:

-   Utilização do processador (CPU);
-   Consumo de memória RAM;
-   Espaço disponível em disco;
-   Tráfego de rede;
-   Disponibilidade de servidores;
-   Funcionamento de serviços;
-   Ocorrências e alertas de falhas.

### Componentes utilizados

  Componente       Função
  ---------------- -------------------------------------------------------------
  Zabbix Server    Recebe, processa e organiza os dados coletados.
  Zabbix Agent 2   Coleta informações do equipamento monitorado.
  MySQL            Armazena configurações, históricos e informações coletadas.
  Apache           Disponibiliza a interface web.
  PHP              Permite executar a interface web do Zabbix.

## 2. Preparação do Ubuntu

Atualize o sistema:

``` bash
sudo apt update && sudo apt upgrade -y
```

Descubra o endereço IP do servidor:

``` bash
hostname -I
```

Anote o endereço IP. Ele será utilizado para acessar o Zabbix pelo
navegador do Windows 11.

## 3. Adicionando o repositório oficial do Zabbix

Baixe o pacote de configuração do repositório:

``` bash
wget https://repo.zabbix.com/zabbix/8.0/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest+ubuntu26.04_all.deb
```

Instale o pacote e atualize a lista de repositórios:

``` bash
sudo dpkg -i zabbix-release_latest+ubuntu26.04_all.deb
sudo apt update
```

## 4. Instalando os componentes

Instale o MySQL, Apache e os pacotes do Zabbix:

``` bash
sudo apt install -y mysql-server apache2 zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent2
```

### Verificação dos pacotes

``` bash
dpkg -l | grep -E 'zabbix-server-mysql|zabbix-frontend-php|zabbix-apache-conf|zabbix-sql-scripts|zabbix-agent2'
```

Os pacotes instalados devem aparecer com o status `ii`.

## 5. Configurando o banco de dados MySQL

### 5.1 Entrar no MySQL

``` bash
sudo mysql
```

### 5.2 Criar o banco e o usuário

Execute os comandos a seguir dentro do prompt do MySQL (`mysql>`):

``` sql
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;

CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'Zabbix@2026!';

GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';

SET GLOBAL log_bin_trust_function_creators = 1;

QUIT;
```

> **Atenção:** estes comandos são destinados a um banco novo. Não os
> execute novamente em uma instalação já configurada. A senha é apenas
> um exemplo para laboratório; em ambientes reais, utilize uma senha
> forte e exclusiva.

### 5.3 Importar a estrutura do Zabbix

Execute no terminal Linux:

``` bash
sudo zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

Quando solicitado, informe a senha definida para o usuário MySQL
`zabbix`:

``` text
Zabbix@2026!
```

Aguarde a conclusão da importação. Ela pode levar alguns minutos e
normalmente não apresenta mensagens quando é concluída com sucesso.

### 5.4 Desabilitar a configuração temporária do MySQL

``` bash
sudo mysql -e "SET GLOBAL log_bin_trust_function_creators = 0;"
```

### 5.5 Verificar a importação

``` bash
mysql -uzabbix -p zabbix -e "SHOW TABLES;"
```

Digite a senha do banco. Deverá aparecer uma lista de tabelas do Zabbix.

> **Importante:** não execute novamente a importação se as tabelas já
> existirem. Se ocorrer uma falha durante a importação, verifique o
> estado do banco antes de tentar novamente.

## 6. Configurando o Zabbix Server

O servidor Zabbix precisa conhecer a senha do banco de dados.

Insira a senha no arquivo de configuração:

``` bash
sudo sed -i 's/^# DBPassword=.*/DBPassword=Zabbix@2026!/' /etc/zabbix/zabbix_server.conf
```

Confira o valor configurado:

``` bash
grep "^DBPassword=" /etc/zabbix/zabbix_server.conf
```

Resultado esperado:

``` text
DBPassword=Zabbix@2026!
```

## 7. Configurando o Apache

Esta etapa merece atenção: o comando `a2enconf zabbix` apenas habilita
uma configuração que já precisa existir. Ele não cria nem instala o
arquivo `zabbix.conf`.

### 7.1 Habilitar o módulo Proxy

``` bash
sudo a2enmod proxy
```

Se aparecer `Module proxy already enabled`, está tudo certo: o módulo já
está habilitado.

### 7.2 Garantir que a configuração do Apache do Zabbix esteja instalada

``` bash
sudo apt install --reinstall zabbix-apache-conf -y
```

Confira se o arquivo existe:

``` bash
ls -l /etc/apache2/conf-available/zabbix.conf
```

**Condição para avançar:** o arquivo `zabbix.conf` precisa aparecer. Se
não aparecer, interrompa esta etapa e verifique a instalação do pacote.

### 7.3 Habilitar a configuração do Zabbix

Somente depois de confirmar que o arquivo existe:

``` bash
sudo a2enconf zabbix
```

Se informar que a configuração já está habilitada, não há problema.

### 7.4 Testar a configuração do Apache

``` bash
sudo apache2ctl configtest
```

Resultado esperado:

``` text
Syntax OK
```

Se aparecer erro relacionado a `ProxyTimeout`, confira se o módulo
`proxy` está habilitado:

``` bash
sudo a2enmod proxy
```

Não reinicie o Apache enquanto o teste de configuração apresentar erros.

### 7.5 Reiniciar o Apache

Somente depois de obter `Syntax OK`:

``` bash
sudo systemctl restart apache2
```

## 8. Iniciando os serviços

Reinicie os serviços:

``` bash
sudo systemctl restart mysql
sudo systemctl restart zabbix-server
sudo systemctl restart zabbix-agent2
sudo systemctl restart apache2
```

Configure a inicialização automática:

``` bash
sudo systemctl enable mysql zabbix-server zabbix-agent2 apache2
```

### 8.1 Verificar se os serviços estão ativos

Execute:

``` bash
sudo systemctl is-active mysql
sudo systemctl is-active zabbix-server
sudo systemctl is-active zabbix-agent2
sudo systemctl is-active apache2
```

Todos devem retornar:

``` text
active
```

## 9. Liberando o acesso pelo firewall

Caso o UFW esteja habilitado, permita o acesso HTTP:

``` bash
sudo ufw allow 80/tcp
```

## 10. Acessando o Zabbix pelo Windows 11

Descubra o IP do Ubuntu:

``` bash
hostname -I
```

No navegador do Windows 11, digite:

``` text
http://IP_DO_SERVIDOR/zabbix
```

Exemplo utilizado no laboratório:

``` text
http://172.29.100.48/zabbix
```

O endereço IP pode mudar no Hyper-V quando se utiliza o Default Switch.
Se isso acontecer, consulte novamente o endereço com `hostname -I`.

## 11. Configurando a interface web

No assistente de instalação, informe os seguintes dados:

  Campo                Valor
  -------------------- --------------
  Database type        MySQL
  Database host        localhost
  Database port        0
  Database name        zabbix
  User                 zabbix
  Password             Zabbix@2026!
  Zabbix server host   localhost
  Zabbix server port   10051

Avance até concluir o assistente.

## 12. Primeiro acesso ao Zabbix

Na tela de login, utilize as credenciais padrão:

**Usuário:**

``` text
Admin
```

**Senha:**

``` text
zabbix
```

> O usuário possui a letra `A` maiúscula. Após entrar, altere a senha
> padrão da administração.

## 13. Conhecendo o painel do Zabbix

Explore as principais áreas da ferramenta:

  -----------------------------------------------------------------------
  Menu                                Funcionalidade
  ----------------------------------- -----------------------------------
  Dashboard                           Visão geral do ambiente monitorado.

  Monitoring                          Acompanhamento dos equipamentos e
                                      informações coletadas.

  Hosts                               Cadastro e gerenciamento dos
                                      equipamentos monitorados.

  Latest data                         Visualização dos dados coletados.

  Problems                            Visualização de problemas e
                                      alertas.

  Reports                             Relatórios de monitoramento.

  Data collection                     Configuração dos itens, hosts e
                                      regras de coleta.
  -----------------------------------------------------------------------

## 14. Atividade prática

Após concluir a instalação, realize as atividades abaixo.

### Atividade 1 -- Conhecendo o ambiente

1.  Acesse o painel do Zabbix.
2.  Identifique a versão instalada.
3.  Localize o menu de monitoramento.
4.  Verifique os equipamentos cadastrados.

### Atividade 2 -- Monitoramento

1.  Acesse **Monitoring → Hosts**.
2.  Verifique se o servidor Ubuntu está cadastrado.
3.  Observe o estado do agente de monitoramento.
4.  Acesse **Monitoring → Latest data**.
5.  Identifique informações relacionadas à CPU, memória e disco, quando
    disponíveis.

### Atividade 3 -- Análise de infraestrutura

Responda:

1.  Qual é a função do Zabbix Server?
2.  Qual é a função do Zabbix Agent 2?
3.  Por que o Zabbix precisa de um banco de dados?
4.  Qual é a função do Apache nessa instalação?
5.  Quais informações podem ser obtidas sobre um servidor utilizando o
    Zabbix?
6.  Qual é a importância de monitorar a disponibilidade dos serviços?
7.  O que pode acontecer quando um servidor apresenta alto consumo de
    memória RAM?

### Atividade 4 -- Simulação de falha

Com orientação do professor:

1.  Identifique um serviço do Ubuntu que esteja sendo monitorado.
2.  Interrompa temporariamente o serviço.
3.  Observe se o Zabbix identifica alguma alteração ou problema.
4.  Inicie novamente o serviço.
5.  Verifique o comportamento do monitoramento.

> Realize a simulação somente em ambiente de laboratório e com
> autorização do professor. Não interrompa serviços essenciais do
> sistema.

## 15. Conclusão

O Zabbix é uma ferramenta importante para administração de
infraestrutura de TI, permitindo acompanhar o funcionamento dos
servidores, identificar problemas e antecipar falhas.

Com esta atividade, foi possível instalar uma solução de monitoramento
em um servidor Linux, configurar seus componentes e disponibilizar uma
interface de administração acessível pela rede.

**Desafio adicional:** pesquise como adicionar um segundo computador
Windows ao Zabbix, utilizando o Zabbix Agent 2, para ampliar o ambiente
de monitoramento.

## Referências

-   Documentação oficial do Zabbix 8.0:
    https://www.zabbix.com/documentation/8.0/pt
-   Site oficial do Zabbix: https://www.zabbix.com/

------------------------------------------------------------------------

## Checklist de conferência da instalação

-   [ ] Ubuntu atualizado.
-   [ ] Repositório oficial do Zabbix instalado.
-   [ ] Pacotes do Zabbix instalados.
-   [ ] Banco MySQL criado.
-   [ ] Tabelas do Zabbix importadas.
-   [ ] Senha do banco configurada.
-   [ ] Arquivo `zabbix.conf` localizado.
-   [ ] Apache validado com `Syntax OK`.
-   [ ] Serviços iniciados e ativos.
-   [ ] Painel web acessível.
-   [ ] Login realizado com sucesso.
