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

Agora, vamos definir uma porta alta na qual o servidor irá escutar para corresponder ao serviço no qual estamos criando:
```bash
vim /etc/apache2/ports.conf
```
Inclua uma linha logo abaixo do parâmetro **Listen 80**

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

Agora, basta criar o diretório de trabalho definido no campo **DocumentRoot**, onde estão localizados as páginas web, programação e scripts. Para isso:
```bash
mkdir -p /var/www/estudos/{html,css,js,scripts,images}
touch /var/www/estudos/index.cgi
chmod +x /var/www/estudos/index.cgi
chown www-data:www-data -R /var/www/estudos
```
**OBS**: Como se tratam de scripts CGI, é importante que os mesmos tenham permissão de execução, uma vez que o Apache o entende como um arquivo executável, caso contrário um erro 500 será retornado, informando erro do lado do servidor.

Agora, basta habilitar o **VirtualHost**:
```bash
a2ensite estudos
systemctl restart apache2
```

A partir deste momento, basta realizar as configurações nos arquivos **.cgi**.

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
        ErrorDocument 401 /html/login.html
    </Directory>
    <Directory "/var/www/surpresa/logout">
        SetHandler form-logout-handler
        AuthFormLogoutLocation "https://surpresa.com.br/"
        SessionMaxAge 1
        Session On
    </Directory>
</VirtualHost>
```
Agora, basta criar o diretório de trabalho definido no campo **DocumentRoot**, onde estão localizados as páginas web, programação e scripts. Para isso:
```bash
mkdir -p /var/www/surpresa/{html,css,js,scripts,images}
touch /var/www/surpresa/index.cgi
chmod +x /var/www/surpresa/index.cgi
chown www-data:www-data -R /var/www/surpresa
```
Defina a página web com elementos básicos de um formulário para validar credenciais de usuários:
```bash
touch /var/www/surpresa/html/login.html
chown www-data:www-data -R /var/www/surpresa
vim /var/www/surpresa/html/login.html
```
```html
<!DOCTYPE html>
<html lang="pt-br">
    <head>
        <meta charset="utf-8">
        <meta name="author" content="Mihguel Da Silva Santos Tavares De Araujo">
        <meta name="description" content="Página de login para curso de shell script com cgi em apache">
        <title>Página de Login</title>
    </head>
    <body>
        <h1>Login</h1>
        <form method="post" action="/" onSubmit="check()">
            <input type="text" name="httpd_username" id="username" placeholder="Insira o seu usuário"> <br>
            <input type="password" name="httpd_password" id="password" placeholder="Insira sua senha"><hr>
            <button type="submit" name="submit" id="submit" value="Login">Login</button>
        </form>
        <script>
            function check() {
                let u = document.getElementById("username");
                let p = document.getElementById("password");
                if (!u || !p) {
                    window.alert("Insira seus dados!") ; return false
                } else {
                    return true
                }
            }
        </script>
    </body>
</html>
```