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
<hr>
<details>
<summary style="cursor: pointer;">Análise de tráfego 3 way handshake</summary>
<pre>
tcpdump -i lo port 80
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on lo, link-type EN10MB (Ethernet), snapshot length 262144 bytes
15:03:14.844328 IP6 localhost.51404 > localhost.http: Flags [S], seq 3701337306, win 65476, options [mss 65476,sackOK,TS val 4186256120 ecr 0,nop,wscale 7], length 0
15:03:14.844395 IP6 localhost.http > localhost.51404: Flags [S.], seq 1827199478, ack 3701337307, win 65464, options [mss 65476,sackOK,TS val 4186256121 ecr 4186256120,nop,wscale 7], length 0
15:03:14.844442 IP6 localhost.51404 > localhost.http: Flags [.], ack 1, win 512, options [nop,nop,TS val 4186256121 ecr 4186256121], length 0 
</pre>
</details>
<hr>

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

<hr>
<details>
  <summary style="cursor:pointer;">Análise de tráfego de requisição/resposta</summary>
  <details>
      <summary style="cursor:pointer;">Resposta do servidor</summary>
      <pre>
HTTP/1.1 200 OK
Date: Fri, 28 Apr 2023 18:10:37 GMT
Server: Apache/2.4.56 (Debian)
Upgrade: h2,h2c
Connection: Upgrade
Last-Modified: Mon, 13 Mar 2023 05:30:25 GMT
ETag: "29cd-5f6c168745bf2"
Accept-Ranges: bytes
Content-Length: 10701
Vary: Accept-Encoding
Content-Type: text/html
      </pre>
  </details>
  <details>
    <summary style="cursor:pointer">Análise de tráfego</summary>
    <pre>
tcpdump -i lo port 80
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on lo, link-type EN10MB (Ethernet), snapshot length 262144 bytes
15:10:33.284885 IP6 localhost.54504 > localhost.http: Flags [S], seq 1609201335, win 65476, options [mss 65476,sackOK,TS val 4186694561 ecr 0,nop,wscale 7], length 0
15:10:33.284937 IP6 localhost.http > localhost.54504: Flags [S.], seq 1114791668, ack 1609201336, win 65464, options [mss 65476,sackOK,TS val 4186694561 ecr 4186694561,nop,wscale 7], length 0
15:10:33.284984 IP6 localhost.54504 > localhost.http: Flags [.], ack 1, win 512, options [nop,nop,TS val 4186694561 ecr 4186694561], length 0
15:10:37.638672 IP6 localhost.54504 > localhost.http: Flags [P.], seq 1:18, ack 1, win 512, options [nop,nop,TS val 4186698915 ecr 4186694561], length 17: HTTP: HEAD / HTTP/1.1
15:10:37.638779 IP6 localhost.http > localhost.54504: Flags [.], ack 18, win 512, options [nop,nop,TS val 4186698915 ecr 4186698915], length 0
15:10:40.566601 IP6 localhost.54504 > localhost.http: Flags [P.], seq 18:34, ack 1, win 512, options [nop,nop,TS val 4186701843 ecr 4186698915], length 16: HTTP
15:10:40.566638 IP6 localhost.http > localhost.54504: Flags [.], ack 34, win 512, options [nop,nop,TS val 4186701843 ecr 4186701843], length 0
15:10:40.701299 IP6 localhost.54504 > localhost.http: Flags [P.], seq 34:36, ack 1, win 512, options [nop,nop,TS val 4186701977 ecr 4186701843], length 2: HTTP
15:10:40.701335 IP6 localhost.http > localhost.54504: Flags [.], ack 36, win 512, options [nop,nop,TS val 4186701978 ecr 4186701977], length 0
15:10:40.701726 IP6 localhost.http > localhost.54504: Flags [P.], seq 1:294, ack 36, win 512, options [nop,nop,TS val 4186701978 ecr 4186701977], length 293: HTTP: HTTP/1.1 200 OK
15:10:40.701755 IP6 localhost.54504 > localhost.http: Flags [.], ack 294, win 510, options [nop,nop,TS val 4186701978 ecr 4186701978], length 0
15:10:45.704961 IP6 localhost.http > localhost.54504: Flags [F.], seq 294, ack 36, win 512, options [nop,nop,TS val 4186706981 ecr 4186701978], length 0
15:10:45.705261 IP6 localhost.54504 > localhost.http: Flags [F.], seq 36, ack 295, win 512, options [nop,nop,TS val 4186706981 ecr 4186706981], length 0
15:10:45.705326 IP6 localhost.http > localhost.54504: Flags [.], ack 37, win 512, options [nop,nop,TS val 4186706981 ecr 4186706981], length 0
    </pre>
  </details>
</details>
<hr>

**OBS**: Este procedimento HTTP será lembrado quando o arquivo CGI principal estiver sendo configurado e desenvolvido. Caso esta regra não seja obedecida, um erro interno referente ao servidor será retornado.

[Básico de Programação](./01-PROGRAMACAO.md) 

[Shell Script CGI com Apache](./03-CGI.md)