# Instruções

Para preparar o ambiente, é preciso ter:

* Servidor GNU/Linux Debian 11;
* Servidor web Apache2 instalado;
* Módulo CGI habilitado;
* Módulos extras habilitados para estudo supresa, caso o tempo requerido seja satisfeito.

**OBS**: Todos os testes foram realizados em um servidor GNU/Linux Debian por sua versatilidade e robustez, além da maneira que lida com os serviços internos e entre outros, logo este mesmo servidor terá foco principal para o estudo do material.

## Instalando servidor web Apache2

```bash
apt-get update && apt-get install apache2 apache2-utils
```

### Habilitando módulo CGI

Uma vez instalado o servidor web Apache2, é importante habilitar os módulos CGI:

```bash
a2enmod cgid
systemctl restart apache2 
```
### Preparando ambiente
A partir deste momento, o material para estudo em laboratório está pronto para ser utilizado. O material a parte, em caso de tempo, necessitará de outros módulos os quais serão alvo de destaque um pouco mais abaixo, todavia, é importante definir as primeiras configurações para o funcionamento em questão:
```bash
vim /etc/apache2/mods-enabled/mime.conf
```
Dentro deste arquivo, procure pelo parâmetro **AddHandler** e descomente-o, informando ao servidor quais tipos de arquivos ele irá lidar e suportar durante a execução de scripts CGI.
```bash
/AddHandler
AddHandler cgi-script .cgi .sh
```
**OBS**: Além de descomentar o parâmetro em questão, ao final foi adicionado a extensão **.sh**, informando ao servidor que além da extensão **.cgi**, o mesmo poderá lidar com **.sh**.

Feito isto, basta criar o **VirtualHost**, mas antes, para uma melhor compreensão da atividade, o isso também será explanado em sala de aula, é bom definir um escopo de nome na resolução local do aluno:
```bash
vim /etc/hosts
127.0.0.1  estudos.com.br
127.0.0.1  surpresa.com.br
::1        estudos.com.br
::1        surpresa.com.br
```
Feito isto, vamos criar os **VirtualHosts**:
```bash
vim /etc/apache2/sites-available/estudos.conf
```
Agora, basta definir os parâmetros:
```bash
<VirtualHost *:80>
    ServerName estudos.com.br
    DocumentRoot /var/www/estudos

    <Directory "/var/www/estudos/">
        Options +ExecCGI
    </Directory>
</VirtualHost>
```
Este é o mínimo para permitir o Apache lidar com diversas linguagens de programação do lado do servidor, e nesta, em especial, Shell Script.

Agora, basta habilitar o **VirtualHost**:
```bash
a2ensite estudos
systemctl restart apache2
```
**OBS**: Como o ambiente deve estar limitado ao diretório home do aluno, basta criar um link simbólico para o diretório /var/www, e o restante das atividades serão realizadas normalmente, focando na base do aluno em questão, e limitando as tarefas no mesmo:
```
mkdir ~/{estudo,surpresa}
ln -s ~/estudo /var/www
ln -s ~/surpresa /var/www
```

### Módulo surpresa
O módulo surpresa irá demonstrar para os alunos montar uma página web simples com o intuito de limitar o acesso a um serviço através de um login. Para isso, será necessário habilitar os módulos:

* mod_auth_form;
* mod_request;
* mod_session;
* mod_session_cookie
```bash
a2enmod session
a2enmod session_cookie
a2enmod request
a2enmod auth_form
systemctl restart apache2
```
Feito isto, basta criar o **VirtualHost** em questão:
```bash
vim /etc/apache2/sites-available/surpresa.conf
```
Agora, basta definir os parâmetros:
```bash
<VirtualHost *:80>
    ServerName surpresa.com.br
    DocumentRoot /var/www/surpresa

    <Directory "/var/www/surpresa/">
        Options +ExecCGI
        require valid-user
        AuthType form
        AuthUserFile /opt/data/.users
        AuthName "Reserved Area"
        Session On
        SessionCookieName session path=/
        ErrorDocument 401 /pages/login.html
    </Directory>
    <Directory "/var/www/surpresa/logout">
        SetHandler form-logout-handler
        AuthFormLogoutLocation "https://surpresa.com.br/"
        SessionMaxAge 1
        Session On
    </Directory>
</VirtualHost>
```