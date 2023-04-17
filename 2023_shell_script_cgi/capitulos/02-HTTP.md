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

### Entendendo o HTTP
O protocolo HTTP pertence a camada de aplicação e utiliza como transporte o TCP. No momento em que um cliente quer fazer uma requisição a um servidor, este estabelece uma conexão TCP antes de enviar/receber mensagens HTTP:

Como exercício para compreender este aspecto de comportamento, vamos abrir dois terminais virtuais, um transmitindo um comando telnet na porta 80 para o servidor web que acabamos de instalar e outro para analisar o tráfego, entendendo passo a passo do que está prestes a ocorrer:
```bash
tcpdump -i lo port 80
```

```bash
telnet localhost 80
```
O último procedimento acima estabelecer uma conexão de três vias TCP com o servidor web. Isso pode ser visto na análise de tráfego. Perceba que não existe nenhuma camada HTTP nessa comunicação, apenas TCP.
```bash
HEAD / HTTP/1.1
Host: localhost

```
Após estabeler a conexão de três vias (three-way handshake), dois sockets *ip:porta* são criados, um do lado do cliente e outro do lado do servidor, ambos com o intuito de trocar mensagens HTTP.

O procedimento acima mostra um cabeçalho de requisição simples HTTP, onde:

1. Na primeira linha é definido o escopo da requisição. Neste escopo três campos são requisitados;
    * O primeiro campo define o método HTTP (GET, POST, PUT, DELETE, HEAD);
    * O segundo define o objeto a ser requisitado;
    * O terceiro define a versão HTTP.
2. A partir da segunda para baixo o escopo do cabeçalho:
    * Host define o hospedeiro no qual os objetos estão armazenados.
3. E existe uma linha em branco após a definição do cabeçalho de requisição HTTP. Essa linha é um procedimento padrão que indica o final do cabeçalho HTTP, transmitindo o mesmo para a requisição.

**OBS**: Este procedimento HTTP será lembrado quando o arquivo CGI principal estiver sendo configurado e desenvolvido. Caso esta regra não seja obedecida, um erro interno referente ao servidor será retornado.

[Inicio](../README.md)

[Básico de Programação](./01-PROGRAMACAO.md)

[Shell Script CGI com Apache](./03-CGI.md)