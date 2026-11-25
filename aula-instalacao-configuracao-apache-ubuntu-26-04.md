# Aula prática – Instalação e configuração do Apache no Ubuntu Server 26.04 LTS

## Objetivos da aula

Ao final desta atividade, o aluno deverá ser capaz de:

- Compreender o que é o Apache HTTP Server e para que ele é utilizado.
- Instalar e verificar o funcionamento do Apache no Ubuntu Server.
- Identificar o endereço IP do servidor e acessá-lo pela rede.
- Reconhecer os principais diretórios e arquivos do Apache.
- Publicar uma página HTML em um servidor web.
- Consultar logs de acesso e de erros.
- Validar a configuração e o estado do serviço.

---

## 1. O que é o Apache?

O **Apache HTTP Server**, geralmente chamado apenas de **Apache**, é um servidor web de código aberto. Ele recebe solicitações feitas por navegadores ou por outros clientes através da rede e responde enviando páginas, imagens, arquivos e outros conteúdos.

Por exemplo, quando um usuário digita um endereço como `http://www.exemplo.com` no navegador, o navegador envia uma solicitação ao servidor web. O Apache recebe essa solicitação, localiza o conteúdo solicitado e o entrega ao navegador.

O Apache é utilizado para:

- Hospedar sites e páginas institucionais.
- Disponibilizar sistemas e aplicações web.
- Publicar páginas HTML, arquivos CSS, JavaScript, imagens e documentos.
- Atuar como servidor web para aplicações que utilizam linguagens como PHP.
- Configurar sites virtuais (*Virtual Hosts*), permitindo hospedar mais de um site no mesmo servidor.
- Implementar comunicação segura por HTTPS, quando configurado com TLS/SSL.

### Apache, HTTP e HTTPS

- **HTTP (Hypertext Transfer Protocol):** protocolo utilizado na comunicação entre navegadores e servidores web. Normalmente utiliza a porta TCP 80.
- **HTTPS (HTTP Secure):** versão protegida do HTTP, que utiliza criptografia TLS. Normalmente utiliza a porta TCP 443.
- **Apache:** é o software servidor web que recebe e processa essas solicitações.

> Nesta atividade, inicialmente utilizaremos HTTP na rede do laboratório. Em ambientes de produção, o acesso a sistemas e informações deve ser protegido com HTTPS.

---

## 2. Ambiente utilizado

Nesta prática, será utilizado:

| Componente | Descrição |
|---|---|
| Sistema operacional do servidor | Ubuntu Server 26.04 LTS |
| Virtualização | Hyper-V |
| Cliente de acesso | Windows 11 |
| Servidor web | Apache HTTP Server |
| Protocolo inicial | HTTP |
| Porta padrão | TCP 80 |
| Página publicada | Intranet demonstrativa do Colégio Sinodal Progresso |

Antes de começar, ligue a máquina virtual Ubuntu Server e acesse o terminal com seu usuário.

É recomendável criar um **Checkpoint** da máquina virtual no Hyper-V antes de realizar alterações importantes.

---

## 3. Atualizar a lista de pacotes

Antes de instalar o Apache, atualize a lista de pacotes disponíveis nos repositórios:

```bash
sudo apt update
```

Esse comando consulta os repositórios configurados no Ubuntu e atualiza as informações sobre os pacotes disponíveis.

---

## 4. Instalar o Apache

Execute:

```bash
sudo apt install apache2 -y
```

O parâmetro `-y` confirma automaticamente a instalação quando o sistema solicitar confirmação.

Após a instalação, confira a versão:

```bash
apache2 -v
```

O terminal deverá apresentar informações sobre a versão instalada do Apache.

---

## 5. Verificar e habilitar o serviço

Confira o estado do serviço:

```bash
sudo systemctl status apache2
```

Procure a indicação:

```text
Active: active (running)
```

Isso significa que o Apache está em execução.

Pressione `q` para sair da tela de status.

Configure o serviço para iniciar automaticamente junto com o Ubuntu:

```bash
sudo systemctl enable apache2
```

Caso seja necessário iniciar o serviço manualmente:

```bash
sudo systemctl start apache2
```

Outros comandos importantes:

```bash
# Reiniciar o serviço
sudo systemctl restart apache2

# Recarregar as configurações sem reiniciar completamente
sudo systemctl reload apache2

# Parar o serviço
sudo systemctl stop apache2

# Consultar novamente o estado
sudo systemctl status apache2
```

---

## 6. Testar o Apache dentro do Ubuntu

Antes de tentar acessar o servidor pela rede, verifique se o Apache está respondendo localmente.

Execute:

```bash
curl http://localhost
```

Se estiver funcionando, o terminal apresentará o código HTML da página padrão do Apache.

Também é possível verificar se existe um processo escutando na porta 80:

```bash
sudo ss -lntp | grep :80
```

O resultado deverá indicar que existe um serviço escutando na porta TCP 80.

> Se o comando `curl` não estiver disponível, instale-o com `sudo apt install curl -y`.

---

## 7. Identificar o endereço IP do servidor

Para descobrir os endereços IP da máquina virtual, execute:

```bash
hostname -I
```

Exemplo de resultado:

```text
172.20.10.5
```

O endereço apresentado será diferente conforme a rede configurada no Hyper-V.

Para consultar as interfaces de rede:

```bash
ip addr
```

Para visualizar a rota padrão:

```bash
ip route
```

Anote o endereço IP correspondente à interface de rede utilizada pela máquina virtual.

### Observação sobre o Hyper-V

O tipo de conexão de rede configurado no Hyper-V influencia a forma como o Windows acessa a VM. Nesta atividade, utilize o endereço IP que permitiu o acesso entre o Windows 11 e o Ubuntu Server durante o teste do laboratório.

---

## 8. Verificar o firewall

Consulte o estado do UFW:

```bash
sudo ufw status
```

Se o firewall estiver **ativo**, libere o acesso HTTP:

```bash
sudo ufw allow 80/tcp
```

Confira as regras:

```bash
sudo ufw status
```

Se o UFW estiver inativo, não é necessário ativá-lo apenas para esta atividade. A comunicação também depende da configuração de rede do Hyper-V e das regras de firewall do Windows.

---

## 9. Acessar o Apache pelo Windows 11

No Windows 11, abra o navegador e digite o endereço IP do servidor:

```text
http://IP_DO_SERVIDOR
```

Exemplo:

```text
http://172.20.10.5
```

Se aparecer a página padrão do Apache, significa que o servidor web está respondendo pela rede.

### Testar a porta pelo Windows

Se a página não abrir, execute no PowerShell:

```powershell
Test-NetConnection IP_DO_SERVIDOR -Port 80
```

Substitua `IP_DO_SERVIDOR` pelo endereço real da VM.

O resultado esperado é:

```text
TcpTestSucceeded : True
```

Se o resultado for `False`, confira:

1. Se o Apache está ativo no Ubuntu.
2. Se o endereço IP digitado está correto.
3. Se o Apache está escutando na porta 80.
4. Se o firewall permite a comunicação.
5. Se a configuração de rede do Hyper-V permite o acesso entre o Windows e a VM.

---

## 10. Conhecer a estrutura de diretórios do Apache

O Apache possui diretórios específicos para armazenar páginas, configurações e registros de funcionamento.

| Diretório | Finalidade |
|---|---|
| `/var/www/html` | Diretório padrão dos arquivos do site |
| `/etc/apache2` | Diretório principal de configurações |
| `/etc/apache2/sites-available` | Configurações dos sites disponíveis |
| `/etc/apache2/sites-enabled` | Configurações dos sites habilitados |
| `/var/log/apache2` | Arquivos de registro (*logs*) |

Para visualizar o diretório padrão das páginas:

```bash
ls -la /var/www/html
```

Para visualizar as configurações:

```bash
ls -la /etc/apache2
```

Para listar os sites disponíveis:

```bash
ls -la /etc/apache2/sites-available
```

Para listar os sites habilitados:

```bash
ls -la /etc/apache2/sites-enabled
```

### Diferença entre sites-available e sites-enabled

- `sites-available`: armazena os arquivos de configuração dos sites que podem ser utilizados pelo Apache.
- `sites-enabled`: contém os sites que estão efetivamente habilitados.

No Ubuntu, normalmente os sites são habilitados com o comando `a2ensite` e desabilitados com `a2dissite`.

---

## 11. Preparar a página da intranet do Sinodal

Para a atividade, será utilizada uma página HTML demonstrativa inspirada no Colégio Sinodal Progresso.

**Arquivo:** `intranet-sinodal-progresso.html`

Faça o download do arquivo HTML disponibilizado junto com este roteiro e salve-o no Windows 11, preferencialmente na pasta Downloads.

A página contém:

- Cabeçalho com identificação do Colégio Sinodal Progresso.
- Navegação entre seções.
- Atalhos demonstrativos para serviços escolares.
- Área de comunicados fictícios.
- Links para o site oficial da escola.
- Informações institucionais e de contato públicas.
- Layout responsivo com CSS integrado ao próprio HTML.

> Esta página é um modelo didático, não um sistema oficial da instituição. Os comunicados e atalhos internos são exemplos.

---

## 12. Instalar e habilitar o serviço SSH

Para transferir o arquivo do Windows para o Ubuntu, utilizaremos o SCP, que permite copiar arquivos por meio de uma conexão SSH.

No Ubuntu, instale o servidor SSH:

```bash
sudo apt install openssh-server -y
```

Habilite e inicie o serviço:

```bash
sudo systemctl enable --now ssh
```

Verifique o estado:

```bash
sudo systemctl status ssh
```

O resultado esperado deverá indicar:

```text
Active: active (running)
```

Pressione `q` para sair.

---

## 13. Transferir o arquivo HTML pelo Windows

No Windows 11, abra o PowerShell.

Execute o comando abaixo, substituindo:

- `SEU_USUARIO` pelo nome da pasta do seu usuário no Windows.
- `USUARIO` pelo usuário cadastrado no Ubuntu.
- `IP_DO_SERVIDOR` pelo endereço IP da máquina virtual.

```powershell
scp "C:\Users\SEU_USUARIO\Downloads\intranet-sinodal-progresso.html" USUARIO@IP_DO_SERVIDOR:/tmp/
```

Exemplo:

```powershell
scp "C:\Users\Silvio\Downloads\intranet-sinodal-progresso.html" aluno@172.20.10.5:/tmp/
```

Na primeira conexão, pode aparecer uma pergunta sobre a autenticidade do servidor SSH. Confira se o endereço está correto e confirme a conexão. Em seguida, informe a senha do usuário do Ubuntu quando solicitado.

Após a transferência, o arquivo estará no diretório `/tmp/` do Ubuntu.

No Ubuntu, confira:

```bash
ls -l /tmp/intranet-sinodal-progresso.html
```

---

## 14. Publicar a página no Apache

O diretório padrão do Apache no Ubuntu é `/var/www/html`.

Copie o arquivo recebido para esse diretório e utilize o nome `index.html`:

```bash
sudo cp /tmp/intranet-sinodal-progresso.html /var/www/html/index.html
```

Ajuste o proprietário:

```bash
sudo chown root:root /var/www/html/index.html
```

Ajuste as permissões:

```bash
sudo chmod 644 /var/www/html/index.html
```

Confira o arquivo:

```bash
ls -l /var/www/html/
```

Deverá aparecer o arquivo `index.html`.

### Entendendo as permissões

O comando `chmod 644` define:

- Proprietário: leitura e escrita.
- Grupo: somente leitura.
- Outros usuários: somente leitura.

Assim, o Apache consegue ler o arquivo e disponibilizá-lo aos visitantes, sem permitir que usuários comuns o modifiquem.

---

## 15. Acessar a intranet pelo navegador

No Windows 11, abra novamente:

```text
http://IP_DO_SERVIDOR
```

Exemplo:

```text
http://172.20.10.5
```

A página padrão do Apache deverá ter sido substituída pela intranet demonstrativa do Sinodal.

Se o navegador continuar mostrando a página anterior, pressione:

```text
Ctrl + F5
```

Esse atalho força a atualização do conteúdo, ignorando o cache do navegador.

---

## 16. Consultar os logs do Apache

O Apache registra informações importantes sobre as solicitações recebidas e os problemas encontrados.

### Log de acessos

Execute:

```bash
sudo tail -f /var/log/apache2/access.log
```

Mantenha o terminal aberto e atualize a página da intranet no navegador do Windows.

Os acessos deverão aparecer no terminal.

Para interromper o acompanhamento, pressione `Ctrl + C`.

### Log de erros

Execute:

```bash
sudo tail -f /var/log/apache2/error.log
```

Esse arquivo registra mensagens relacionadas a erros e problemas de execução ou configuração.

Para sair, pressione `Ctrl + C`.

---

## 17. Validar a configuração do Apache

Execute:

```bash
sudo apache2ctl configtest
```

O resultado esperado é:

```text
Syntax OK
```

Essa mensagem indica que a sintaxe dos arquivos de configuração foi validada.

Confira o estado final do serviço:

```bash
sudo systemctl status apache2
```

O resultado deverá indicar:

```text
Active: active (running)
```

---

## 18. Atividade prática para os alunos

### Atividade – Instalação e publicação de um site com Apache

Após acompanhar a demonstração, responda às questões:

1. O que é o Apache HTTP Server?
2. Para que um servidor web é utilizado?
3. Qual comando permite verificar a versão do Apache instalada?
4. Qual comando permite verificar se o Apache está em execução?
5. Qual é a porta padrão utilizada pelo protocolo HTTP?
6. Qual é o diretório padrão para armazenar páginas web no Ubuntu?
7. Onde ficam armazenados os arquivos de configuração do Apache?
8. Qual é a diferença entre `sites-available` e `sites-enabled`?
9. Qual é a finalidade dos arquivos `access.log` e `error.log`?
10. Qual comando permite verificar se a configuração do Apache possui erros?
11. Qual é a diferença entre HTTP e HTTPS?
12. Por que é importante utilizar HTTPS em sites e sistemas que transmitem informações sensíveis?

### Desafio prático

Modifique a página `index.html`, adicionando:

- Nome da turma.
- Uma mensagem de boas-vindas.
- Uma seção com informações sobre o laboratório de informática.

Depois, atualize a página no navegador do Windows e confira o resultado.

---

## 19. Checklist de conclusão

Utilize esta lista para conferir se todas as etapas foram concluídas:

- [ ] Apache instalado.
- [ ] Versão do Apache identificada.
- [ ] Serviço Apache ativo.
- [ ] Inicialização automática habilitada.
- [ ] Teste local com `curl http://localhost` realizado.
- [ ] Endereço IP do servidor identificado.
- [ ] Firewall verificado.
- [ ] Página padrão acessada pelo Windows 11.
- [ ] Estrutura de diretórios do Apache identificada.
- [ ] Serviço SSH instalado e funcionando.
- [ ] Arquivo HTML transferido por SCP.
- [ ] Página da intranet publicada em `/var/www/html`.
- [ ] Logs de acesso e erro consultados.
- [ ] Configuração validada com `Syntax OK`.

---

## 20. Resultado esperado

Ao final da prática, teremos o seguinte ambiente:

```text
Windows 11
    |
    | Navegador web
    | http://IP_DO_SERVIDOR
    |
    v
Ubuntu Server 26.04 LTS
    |
    v
Apache HTTP Server
    |
    | Porta TCP 80
    |
    v
/var/www/html/index.html
    |
    v
Intranet demonstrativa do Sinodal Progresso
```

O servidor estará instalado e funcionando, disponibilizando a página HTML para acesso pelo navegador de outro computador da rede.

**Próximos assuntos que podem ser trabalhados em aula:**

- Configuração de Virtual Hosts para hospedar mais de um site.
- Configuração de HTTPS com certificado TLS.
- Publicação de aplicações web.
- Controle de acesso e permissões.
- Monitoramento dos serviços e análise dos logs.

---

**Material didático – Arquitetura e Configuração de Servidores**  
**Colégio Sinodal Progresso**
