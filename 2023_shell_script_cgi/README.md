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

Após realizar tal procedimento, é importante dar permissões de execução, uma vez que é um arquivo executável:
```bash
chmod +x arquivo.sh
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

Sendo um arquivo executável, é possível executar o mesmo de duas maneiras:

* Definindo o caminho relativo;
* Definindo o caminho absoluto;
* Os anteriores executados pelo programa bash.

```bash
./arquivo.sh  # precisa estar no mesmo diretório de trabalho
/home/aluno/arquivo.sh  # caso o arquivo esteja neste caminho
bash ./arquivo.sh  # precisa estar no mesmo diretório de trabalho
bash /home/aluno/arquivo.sh  # caso o arquivo esteja neste caminho
```

Além das variáveis armazenarem valores em memória, é possível armazenar um comando que devolva um resultado:
```bash
#!/bin/bash
COMANDO=$(cut -d ":" -f1 /etc/passwd | egrep "^ro")
echo "O usuário selecionado é ${COMANDO}"
```

**OBS**: Shell script possui tipagem fraca e dinâmica:

* **Tipagem Fraca**: Permite a concatenação de diferentes tipos primitivos, sem se importar quais são;
* **Tipagem Dinâmica**: Permite alterar o valor de variáveis ao longo do script.

#### Comentários
Comentários são textos que dão senso de direção ao programador. Estes textos não são interpretados pelo interpretador, ignorando os mesmos.
```bash
#!/bin/bash
# Variável que recebe um nome
NOME="mihguel"
# Variável que recebe um comando
COMANDO=$(cut -d ":" -f1 /etc/passwd | egrep "^ro")
# Variável que recebe um número
NUM=1
# Variável que faz a soma da variável NUM com 2
SUM=$(($NUM + 2))
# Retornar resultado das variáveis em contexto
echo "O resultado da variável NUM mais dois é ${SUM}, pois o cálculo é ${NUM} + 2"
echo "O nome do usuário é ${NOME} e o root dele é ${COMANDO}"
```

#### Entrada de dados
É possível e interessante criar um contexto no qual o usuário consiga se comunicar com o programa. Para isso são utilizados as entradas de dados. O comando responsável por fazer isso são o conjunto **echo** e **read**.

**OBS**: Caso o programador deseje, é possível utilizar apenas o comando **read** seguido pelo parâmetro *-p*.

```bash
#!/bin/bash
echo "Qual o seu nome? "
read NOME
echo "Olá ${NOME}"
read -p "Qual a sua idade? " IDADE
echo "${NOME} tem ${IDADE} anos"
```
Segundo o campo de ajuda do comando read:
> Lê uma linha da entrada padrão e separa em campos.

Para entender melhor o funcionamento do comando **read**:
```bash
read --help
```

##### Fluxo de dados
Para entender melhor o que foi feito acima, vamos explorar o fluxo de dados:

Durante uma operação, o usuário pode:

* Enviar dados para o terminal. Essa seria a entrada padrão (stdin);
* O sistema operacional pode retornar uma informação para o usuário. Essa seria a saída padrão (stdout);
* Durante uma operação, um erro pode ser retornado. Essa seria a saída de erros (stderr).

No GNU/Linux, cada fluxo tem a sua representação:

* **stdin**: 0;
* **stdout**: 1;
* **stderr**: 2.

##### Redirecionar saídas
Antes de exemplificar fluxo de dados, vamos entender o redirecionamento.

Toda a saída de um comando é direcionada para o terminal, mas é possível direcionar a sua saída para um arquivo. Dessa maneira:

* **>**: Direciona a saída para algo, criando um arquivo ou reescrevendo todo o conteúdo no mesmo;
* **>>**: Direciona a saída para algo, criando um arquivo ou adicionando todo o conteúdo no mesmo.
* **|**: Recebe a saída de um comando e joga para a entrada de outro.

```bash
ls > listar.txt
ls / > listar.txt
```
No procedimento acima, o que aconteceu?

```bash
ls /etc >> listar.txt
```
E agora, o que aconteceu?
```bash
ls /etc | grep passwd
```
O que foi retornado?

Vamos entender o que aconteceu:

* No primeiro exemplo foi criado um arquivo listar, onde a saída do conteúdo foi adicionada, e em seguida, um segundo comando foi executado, onde a saída reescreveu completamente o arquivo;
* No segundo exemplo, a saída do comando atualizou o arquivo, não reescrevendo o mesmo;
* No terceiro exemplo, a saída do primeiro comando foi jogado para a entrada do segundo comando, onde o resultado de listar o diretório de configuração passou por uma filtragem, na tentativa de encontrar alguma coisa que contenha "passwd".

Essa versatilidade de jogar a saída de um comando para a entrada de outro torna o bash tão poderoso!

##### Manipulando fluxos de dados
Entendido o funcionamento de redirecionamentos, vamos manipular os fluxos:

```bash
ls /home 2> error.txt 1>saida.txt
```
No exemplo acima, perceba que a saída de erros foi jogado para um arquivo e a saída padrão para outro. Através deste procedimento é possível criar arquivos de logs, definir escopos de procedimento para análise futura e entre outros.

##### Resumo entrada de dados
O read capta a entrada padrão após o Sistema Operacional, através de um script de shell transmitir na sua saída uma pergunta a fim de interagir com o usuário final. Após primeira interação, o read armazena o valor em uma variável para utilização posterior.

#### Vetores
Em linguagens de programação, uma variável ocupa um espaço em memória para armazenar um valor. Como este espaço é limitado, preencher um código com diversas variáveis pode tornar sua execução não funcional.

Para isso, existem os vetores ou variáveis compostas. Elas são responsáveis por armazenar mais de um valor, representando cada um através de um índice.

**OBS**: Na programação, tudo inicia a partir do zero, sendo assim, os índices que representam os valores também o serão.

**OBS**: É importante que o programador tenha ciência do que uma variável representa, sendo assim, o nome da mesma é importante. Não é obrigatório definir um nome seguindo um contexto, mas recomendável.

```bash
#!/bin/bash
NOME=("mihguel" "valdir" "joao" "maria" "andre" "paulo")
echo $NOME  # retorna o primeiro valor
echo ${NOME[@]}  # retorna todos os valores
echo ${#NOME[@]}  # retorna número de índices
echo ${NOME[0]}  # mihguel
echo ${NOME[1]}  # valdir
echo ${NOME[-1]}  # paulo
echo ${NOME[-2]}  # andre
```

#### Condicionais
Num script, para tomar certas decisões, você pode definir uma ação direta ou utilizar uma tabela verdade para tal.

##### Operadores
Na progração existem alguns operadores:

1. Operador de atribuição;
	* O sinal *=* não significa igualdade, mas recebe, logo NOME recebe mihguel.
* Operadores aritméticos;
	* Operadores matemáticos que todos conhecemos;
* Operadores lógicos;
	* Através da comparação de sentenças, será definido se a mesma é verdadeira ou falsa.
* Operadores de comparação;
	* Realizam comparações de diversas maneiras.
* Operadores de associação.
	* Verifica se um elemento está contido em outro.

**OBS**: A base teórica é importante, apesar do conhecimento aprofundado em operadores ser mais necessário em linguagens de programação mais sofisticadas como no caso do Python e Javascript.

#### Test
Segundo o manual:
> Verifica tipos de arquivos e compara valores.

A última definição será explorada. Como mencionado sobre a tabela verdade, iremos definir sentenças e verificar se as mesmas são verdadeiras ou falses, e baseado nisso, é possível utilizar as condicionais.

1. Vamos atravessar a rua.
	* Se um carro aparecer, fico parado;
	* E se uma moto aparecer, fico parado;
	* E se o sinal estiver vermelhor, fico parado.
2. Caso contrário, atravesso a rua.

Perceba no exemplo acima que para tomar uma decisão, algumas condição precisa ser satisfeitas. Neste exemplo:

| **Sentença**  | **Tabela Verdade** | **Ação**  |
|---------------|:------------------:|-----------|
|  Vem carro    |      false         | Parado    |
|  Vem moto     |      false         | Parado    |
| Sinal vermelho|      false         | Parado    |
| Sinal verde   |       true         | Atravessar|

Essa verificação é importante no desenvolvimento dos scripts. 
```bash
#!/bin/bash
read -p "Qual o seu nome? "NOME
```
No script acima, vamos definir uma condicional. Caso o nome não seja Mihguel, um erro será retornado *(tabela verdade retorna falso)*, informando que o acesso foi negado!
```bash
#!/bin/bash
read -p "Qual o seu nome? " NOME
if test $NOME == "Mihguel"
then
    echo "Você tem permissão para acessar o serviço"
else
    echo "Você não tem permissão para acessar o serviço"
fi
```
Neste exemplo, se a condição definida for satisfeita, a tabela verdade irá retornar verdadeiro *(true)*, caso contrário, irá retornar falso *(false)*.

A estrutura condicional se *(if)*, define um bloco de código que será executado caso uma determinada condição seja satisfeita. Esse bloco de código define uma alteração de comportamento no seu script.

```bash
#!/bin/bash
read -p "Qual o seu nome? " NOME
if [[ $NOME == "Mihguel" ]];then
    echo "Usuário pode acessar o sistema"
elif [[ $NOME == "Joao" ]];then
    echo "Usuário pode apenas ler um conteúdo específico"
elif [[ $NOME == "Marcos" ]];then
    echo "Usuário pode apenas realizar manutenção no sistema"
else
    echo "Você não tem nenhuma permissão!"
fi
```
As condicionais permitem criar diversas sentenças. Também é possível criar condicionais aninhadas. Nesse caso, uma condição será verificada após a anterior ter sido satisfeita:
```bash
#!/bin/bash
read -p "Qual o seu nome? " NOME
if test $NOME == "Mihguel"
then
    read -p "Qual a sua senha? " SENHA
    if test $SENHA == "Teste123"
    then
        echo "Usuário tem acesso ao sistema!"
    else
        echo "Senha incorreta para usuário $NOME"
    fi
else
    echo "Usuário não permissão para acessar o sistema!"
fi
```
Perceba que na condição aninhada, o erro retornado é diferente da anterior, caso a tabela verdade não seja satisfeita!

**OBS**: Em programação, o sinal de igualdade é representado com *==*. Lembre-se, *=* é um operador de atribuição, e se lê *variável X recebe o valor de Y*!

**OBS 1**: O comando test também pode ser executado através dos parâmetros *[[]]* ou *(())*. Dependendo da distribuição, essa representação pode mudar.

#### Laços de Repetição
É possível que uma determinada tarefa seja repetida diversas vezes. Um exemplo seria criar um contexto para os usuários existentes no arquivo */etc/passwd*. Uma opção para isso seria:
```bash
cut -d ":" -f1 /etc/passwd
```
Uma vez que foi retornado no nome de cada usuário, é possível que o desenvolvedor selecione, um a um, e crie um contexto. Porém, isso além de não ser produtivo é penoso e cria um vínculo de dependência com o desenvolvedor. Lembre-se que uma das tarefas é automatizar, e para isso podemos utilizar os laços de repetição.

Um laço de repetição define uma condição na qual um conjunto de passos será repetido até que a sentença seja satisfeita.
```bash
#!/bin/bash
USUARIO=$(cut -d ":" -f1 /etc/passwd)
for i in $USUARIO;do
    if test $i == "lab"
    then
        echo "Usuário encontrado!"
    fi
done
```
Outra maneira de criar um laço de repetição:
```bash
#!/bin/bash
CONTADOR=0
while [[ $CONTADOR < 9 ]];do
    echo $CONTADOR
    let CONTADOR++
done
```
O exemplo abaixo pode ser reconhecido por muitos programadores por se assemelhar com algumas linguagens de programação:
```bash
#!/bin/bash
for ((A=0; A<9;A++));do
    echo $A
done
```
#### Funções
Funções agregam um bloco de código que pode ser utilizado diversas vezes dentro de um script de programa. Como um procedimento pode se repetir diversas vezes em contextos diferentes, as funções servem para dar ênfase ao mesmo:
```bash
#!/bin/bash
function senha {
    read -p "Qual a sua senha? " SENHA
    if test $NOME == "Mihguel" -a $SENHA == "Teste123"
    then
        echo "Usuário pode acessar o sistema!"
    elif test $NOME == "Joao" -a $SENHA == "Jaca49"
    then
        echo "Usuário de somente leitura!"
    elif test $NOME == "Karen" -a $SENHA == "Rebeca98"
    then
        echo "Usuário de manutenção!"
    else
        echo "Credenciais incorretas ou usuário inválido!"
    fi
}
read -p "Qual o seu nome? " NOME
senha
```
Apesar de existir apenas um contexto no script, este bloco de código estará disponível, definido um conjunto de passos a serem seguidos, permitindo a criação de novos contextos:
```bash
#!/bin/bash
function senha {
    read -p "Qual o seu nome? " NOME
    read -p "Qual a sua senha? " SENHA
    if test $NOME == "Mihguel" -a $SENHA == "Teste123"
    then
        echo "Usuário pode acessar o sistema!"
        /bin/true
    elif test $NOME == "Joao" -a $SENHA == "Jaca49"
    then
        echo "Usuário de somente leitura!"
        /bin/true
    elif test $NOME == "Karen" -a $SENHA == "Rebeca98"
    then
        echo "Usuário de manutenção!"
        /bin/true
    else
        echo "Credenciais incorretas ou usuário inválido!"
        /bin/false
    fi
}
function checar {
    if test $? -ne 0
    then
        echo "$ENV inacessível"
    else
        echo "$ENV acessível"
    fi
}
function app {
    if test $APP -eq 1
    then
        senha
        ENV="Serviço google"
        checar
    elif test $APP -eq 2
    then
        echo "Acessando jogos online"
        /bin/true
        ENV="Serviço click jogos"
        checar
    elif test $APP -eq 3
    then
        senha
        ENV="Serviço mobile"
        checar
    fi
}
echo "Qual serviço você deseja acessar? "
echo "1. Serviço google"
echo "2. Click jogos"
echo "3. Serviço mobile"
read APP
app
```
Vamos tornar o código mais sofisticado utilizando um arquivo de texto para armazenar usuário e senha:
```bash
touch users.txt
```
Este será um arquivo simples, onde teremos *USUARIO:SENHA:PERMISSAO*, assim as credenciais não estarão dispostas no código:
```bash
Mihguel:Teste123:administrador
Valdir:CredenceWater:leitura
Marcos:Castro95:manutenção
Vivaldi:9192H98:leitura
Carlos:AlbertoNobrega:operador
Maria:Eduarda909568:leitura
```
Vamos fazer algumas alterações no código:
```bash
#!/bin/bash
function checar {
    if test $? -ne 0
    then
        echo "$ENV inacessível"
    else
        echo "$ENV acessível"
    fi
}
function senha {
    read -p "Usuario " NOME
    PASS=$(grep -nw $NOME $PWD/users.txt | cut -d ":" -f3)
    PERM=$(grep -nw $NOME $PWD/users.txt | cut -d ":" -f4)
    if test ! -z $PASS
    then
        read -p "Qual a sua senha? " SENHA
        if test $SENHA == $PASS
        then
            echo "Usuário $PERM"
            /bin/true
        else
            echo "Senha não confere com usúario $NOME"
            /bin/false
        fi
    else
        echo "Usuario $NOME não existe!"
        /bin/false
    fi
}
function app {
    if test $APP -eq 1
    then
        ENV="Serviço google"
        senha; checar
    elif test $APP -eq 2
    then
        echo "Acessando jogos online"
        /bin/true
        ENV="Serviço click jogos"
        checar
    elif test $APP -eq 3
    then
        ENV="Serviço mobile"
        senha; checar
    fi
}
echo "Qual serviço você deseja acessar? "
echo "1. Serviço google"
echo "2. Click jogos"
echo "3. Serviço mobile"
read APP
app
```
Nesta fase de nosso código, perceba que todo o processo foi automatizado, que novos usuários pode ser adicionados ou removidos sem que o script seja alterado, melhorando o ambiente de execução.

Vamos criar uma nova funcionalidade. Vamos gerenciar usuaŕios!
```bash
#!/bin/bash
function checar {
    if test $? -ne 0
    then
        echo "$ENV inacessível"
    else
        echo "$ENV acessível"
    fi
}
function checar_gerenciamento_usuarios {
    if test -z $NOME 2> /dev/null || -z $SENHA 2> /dev/null || -z $PERM  2> /dev/null
    then
        echo "Você não preencheu o campo requisitado!"
        exit 1
    fi
    if test $NOME == $(grep -w $NOME $PWD/users.txt | cut -d":" -f1) 2> /dev/null
        then
        echo "Usuário já existe!" ; exit 0
    fi
}
function checar_permissao {
    if [[ $PERM =~ ^(leitura|administrador|manutenção)$ ]];then
        echo "Usuário criado com sucesso!"
    else
        echo -e "Permissão inexistente!\nEscolha uma permissão:\n- administrador\n- manutenção\n- leitura"
        exit 1
    fi
}
function gerenciar_usuarios {
    echo "Para gerenciar usuários:"
    echo "1. Criar usuários"
    echo "2. Deletar usuários"
    read MANAGE
    if test $MANAGE -eq 1 -o $MANAGE -eq 2 2> /dev/null
    then
        if test $MANAGE -eq 1
        then
            read -p "Qual o nome do novo usuário? " NOME
            checar_gerenciamento_usuarios
            read -p "Qual a senha do novo usuário? " SENHA
            checar_gerenciamento_usuarios
            read -p "Qual a permissão do novo usuário? " PERM
            checar_permissao
            echo "$NOME:$SENHA:$PERM" >> $PWD/users.txt
        else
           read -p "Qual o usuaŕio que você deseja deletar? " NOME
           NUMBER=$(grep -nw $NOME $PWD/users.txt | cut -d ":" -f1)
           if test -z $NUMBER
           then
               echo "Usuário não existe!"
           else
               sed -i "${NUMBER}d" $PWD/users.txt
               echo "Usuário deletado com sucesso!"
           fi
        fi
    else
        echo "Opção inválida!"
    fi
}
function senha {
    read -p "Usuario " NOME
    PASS=$(grep -nw $NOME $PWD/users.txt | cut -d ":" -f3)
    PERM=$(grep -nw $NOME $PWD/users.txt | cut -d ":" -f4)
    if test ! -z $PASS
    then
        read -p "Qual a sua senha? " SENHA
        if test $SENHA == $PASS
        then
            echo "Usuário $PERM"
            /bin/true
        else
            echo "Senha não confere com usúario $NOME"
            /bin/false
        fi
    else
        echo "Usuario $NOME não existe!"
        /bin/false
    fi
}
function app {
    if test $APP -eq 1
    then
        ENV="Serviço google"
        senha; checar
    elif test $APP -eq 2
    then
        echo "Acessando jogos online"
        /bin/true
        ENV="Serviço click jogos"
        checar
    elif test $APP -eq 3
    then
        ENV="Serviço mobile"
        senha; checar
    elif test $APP -eq 4
    then
        gerenciar_usuarios
    else
        echo "Opção inválida!"
    fi
}
echo "Qual serviço você deseja acessar? "
echo "1. Serviço google"
echo "2. Click jogos"
echo "3. Serviço mobile"
echo "4. Gerenciar Usuários"
read APP
app
```

#### Variáveis de ambiente
O bash contém diversas variáveis de ambiente e que foram utilizadas no script. Para saber quais são elas:
```bash
env
```

#### Código de saída
Para que o script funcionasse adequadamente, códigos de saída foram manipulados. De maneira geral:

* **Código 0**: Quando um procedimento ou comando é executado corretamente;
* **Código diferente de 0**: Quando um procedimento ou comando não é executado corretamente. 


# Referências Bibliográficas

SIMULANDO programação orientada a objeto. [S. l.], 15 dez. 2016. Disponível em: https://www.shellscriptx.com/2016/12/simulando-programacao-orientada-a-objeto.html. Acesso em: 14 abr. 2023.

SHELL Script. [S. l.], 2013. Disponível em: http://www.inf.ufes.br/~vitorsouza/archive/2020/wp-content/uploads/teaching-lp-20132-seminario-shell.pdf. Acesso em: 13 abr. 2023.

HIRA, Zaira. Grep Command in Linux – Usage, Options, and Syntax Examples. [S. l.], 17 jan. 2023. Disponível em: https://www.freecodecamp.org/news/grep-command-in-linux-usage-options-and-syntax-examples/. Acesso em: 14 abr. 2023.

SHELL script. [S. l.], 27 mar. 2022. Disponível em: https://mange.ifrn.edu.br/shell-script-wikipedia/. Acesso em: 13 abr. 2023.

INTRODUÇÃO ao Linux e Programação em Shell-Script. [S. l.], 2004. Disponível em: https://www.telecom.uff.br/pet/petws/downloads/apostilas/LINUX.pdf. Acesso em: 14 abr. 2023.




