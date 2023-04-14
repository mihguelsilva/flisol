# Shell Script CGI com Apache

## Programação
Um algoritmo é um conjunto de passos que são passados para o computador para que uma tarefa seja realizada. Imagine o procedimento para atravessar a rua:

1. Aproximar-se da calçada;
2. Olhar para os dois lados;
3. Certificar-se que nenhum carro está se aproximando;
4. Atravessar para o outro lado da rua.

**OBS**: Nesse pequeno conjunto de passos, perceba que a ação é totalmente dependente de diversos fatores, e que para cada ocasião existe uma possibilidade de se locomover ou permanecer parado. Este procedimento será alvo de estudos mais a frente.

### Shell Script
O Shell Script é uma linguagem de programação simples e direta que utiliza como interpretador o **bash** *(Bourne Again Shell)*. Além de linguagem de programção, o bash também é o ambiente no qual o usuário interage com o Sistema Operacional.

> Interpretadores são programas de computador que leem um código fonte de uma linguagem de programação interpretada e o converte em código executável. Seu funcionamento pode variar de acordo com a implementação.

Para indicar que um arquivo deve ser interpretado pelo bash, é preciso definir o interpretador no início do programa. Este procedimento é denominado de **shebang**.
```bash
#!/bin/bash
```

A partir deste momento, todo o procedimento realizado dentro deste arquivo será interpretado pelo shell. Caso algum erro ocorra, será retornado ao usuário.

**OBS**: No primeiro contato, será utilizado o comando echo, que segundo o próprio manual:

> echo retorna uma linha de texto.

Para entender melhor o funcionamento do comando echo:
```bash
man echo
```

#### Operação simples
Nessa primeira operação, será realizado uma soma:
```bash
echo $((1 + 1))
```
Nessa operação simples, perceba a limitação definida pelo script, pois ambos os números nunca vão mudar.

#### Variáveis
Variáveis são responsáveis por armazenar um valor na memória, definindo um contexto para ser utilizado posteriormente. No **bash** não existe um parâmetro que especifica a declaração de uma variável. Seu procedimento é semelhante ao da linguagem de programação **Python**.
```bash
#!/bin/bash
NOME="mihguel"
```
Como exemplificação, abaixo serão destacados a declaração de variáveis em algumas linguagens de programação:
##### Python
```python
nome = "mihguel"
```
##### Javascript
```javascript
const nome = "mihguel"  # variável constante
var nome = "mihguel"  # variável global
let nome = "mihguel"  # variável de escopo
```
**OBS**: As variáveis não podem conter espaços entre o sinal de atribuição *=* e o valor, caso contrário, o **bash** irá interpretar um comando a parte.
```bash
#!/bin/bash
NOME = "mihguel"  # errado
NOME ="mihguel"  # errado
NOME= "mihguel"  # errado
NOME="mihguel"  # certo
```
Para retornar o valor de uma variável no script, basta utilizar o parâmetro $ em conjunto com o comando **echo**.
```bash
#!/bin/bash
NOME="mihguel"
echo "Meu nome é ${NOME}"
```
**OBS**: Para retornar valores de variáveis são utilizadas as aspas duplas. As aspas simples retornam o valor literal, ou seja, *Meu nome é ${NOME}*.

Além das variáveis armazenarem valores em memória, é possível armazenar um comando que devolva um resultado:
```bash
COMANDO=$(cut -d ":" -f1 /etc/passwd | egrep "^ro")
echo "O usuário selecionado é ${COMANDO}"
```

Quando trabalhamos com variáveis, trabalhamos com tipos de dados. No Shell Script eles podem ser:

* **String**: Textos, NOME="Olá mundo",
* **Number**


# Referências Bibliográficas

SIMULANDO programação orientada a objeto. [S. l.], 15 dez. 2016. Disponível em: https://www.shellscriptx.com/2016/12/simulando-programacao-orientada-a-objeto.html. Acesso em: 14 abr. 2023.

SHELL script. [S. l.], 27 mar. 2022. Disponível em: https://mange.ifrn.edu.br/shell-script-wikipedia/. Acesso em: 13 abr. 2023.




