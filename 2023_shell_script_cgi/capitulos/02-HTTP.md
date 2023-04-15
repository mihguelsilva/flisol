[Inicio](../README.md)

# HTTP

Agora que temos um entendimento básico sobre Shell Script, vamos compreender como funciona o básico do HTTP para que montarmos nosso servidor web.

## Primeiros passos
Acesse o usuário root para obter acesso administrativo ao servidor:
```bash
su
```
Descomente o repositório correspondente ao CD/DVD:
```bash
nano /etc/apt/sources.list
```
```bash
# deb cdrom:[Debian GNU/Linux 11.6.0 _Bullseye_ - Official amd64 DVD Binary-1 20221217-10:40]/ bullseye contrib main
```
Defina os programas a estarem no caminho dentro da variável $PATH:
```bash
nano /etc/profile.d/path.sh
```
```bash
#!/bin/bash
PATH="${PATH}:/sbin"
```
Saia da sessão root e retorne novamente para que as alterações sejam aplicadas:
```bash
exit
su
```

## Instalando servidor web Apache2

```bash
apt-get update && apt-get install apache2 apache2-utils tcpdump nmap links2
```

[Inicio](../README.md)

[Básico de Programação](./01-PROGRAMACAO.md)

[Shell Script CGI com Apache](./03-CGI.md)