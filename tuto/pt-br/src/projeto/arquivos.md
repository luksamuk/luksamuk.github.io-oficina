# O arquivo "main.cpp"

Navegue até `MeuJogo/src` e crie um arquivo `main.cpp` (no Linux, você pode navegar até a pasta com um terminal e digitar `touch main.cpp` para criar este arquivo. Ou pode simplesmente inserir o código no seu editor de texto preferido e salvá-lo nesta pasta). Insira neste arquivo o seguinte código:

```cpp
#include <oficina2/oficina.hpp>
using namespace oficina;

int main(int argc, char** argv)
{
    ofInit();
    ofGameLoop();
    ofQuit();
    return 0;
}
```

Neste código, começamos incluindo o cabeçalho C++ `oficina2/oficina.hpp`. Este é o cabeçalho geral da OficinaFramework que contém a maioria dos símbolos e definições que precisamos para criar uma aplicação rápida. Logo após, deixamos claro que usaremos símbolos do namespace `oficina`, onde absolutamente todas as funções, classes, enumerações e structs da engine estão definidos.

O uso do `argc` e do `argv` na função `main` é essencial, por requerimento da biblioteca SDL2, que usaremos para criação e gerenciamento da janela e de seus eventos.

A função `ofInit` inicializa a janela e um contexto de desenho automaticamente, sem que precisemos instanciar os pormenores da engine. Esta função também aceita argumentos, que veremos a seguir.

A função `ofGameLoop` realiza o loop do jogo, onde ocorrerá toda a lógica de atualização de quadros (frames) do jogo, bem como toda a lógica dos objetos e entidades do jogo em geral. É imprescindível salientar que quaisquer telas ou objetos a serem inseridos no jogo devem, preferencialmente, serem adicionados DEPOIS de `ofInit` e ANTES de `ofGameLoop`. Isto exemplifica-se ao criarmos um Canvas de desenho, a ser visto na próxima seção.

A função `ofQuit` executa toda a lógica de limpeza do jogo após o fim da execução do mesmo, ao ser encerrado. JAMAIS chame esta esta função dentro de uma lógica a ser executada dentro de `ofGameLoop` (como num método de atualização de um Canvas, por exemplo). Ao invés disso, uma bandeira para finalização do jogo pode ser levantada ao chamar a função `ofSoftStop`, a ser vista também em um outro momento.

Este código bem básico vai assegurar que consigamos executar uma tela de desenho simples e sem absolutamente nada, mas será o suficiente para um início. Agora, vamos tratar de compilar este arquivo para que se torne o nosso jogo futuramente.

# O arquivo "Makefile"

Agora, navegue até a pasta raiz do jogo (`MeuJogo`) e crie um arquivo chamado `Makefile` (sem extensões).

Caso você não esteja familiarizado com Makefiles, estes arquivos são scripts de compilação muito usados no Linux, feitos para facilitar e automatizar a compilação de projetos. Não são muito avançados no sentido de que boa parte da configuração feita é crua mas, como estamos tratando de um projeto pequeno, Makefiles nos servirão bem.

Abra este arquivo e coloque o seguinte texto. ATENÇÃO: Note que as indentações, após as linhas terminadas com ":", devem ser feitas com O CARACTERE DE TABULAÇÃO (tecla Tab), e não com espaços; do contrário, a compilação mostrará erros.


```Makefile
CXX      = g++ --std=c++11
CXXFLAGS = -g -Wall `oficina2-config --cppflags`
CXXLIBS  = `oficina2-config --libs`
CXXOUT   = -o
CXXOBJ   = -c

DEL      = rm -rf

BIN      = bin/MeuJogo
FILES    = src/main.cpp

.PHONY: clean

all: $(FILES)
        $(CXX) $(FILES) $(CXXFLAGS) $(CXXLIBS) $(CXXOUT) $(BIN)

clean:
        $(DEL) obj/*.o
```

Aqui definimos, antes de mais nada, alguns símbolos (macros), sendo eles:

- `CXX`: Denomina o compilador que usaremos. Aqui usaremos, necessariamente, o compilador GCC, com especificações de C++11. Estas especificações também são automaticamente ativadas ao inserirmos as flags de compilação da OficinaFramework, mas aqui as adicionamos para manter a clareza. Caso você prefira usar o compilador Clang, basta substituir `g++` por `clang++`.
- `CXXFLAGS`: Flags de compilação a serem usadas. Aqui adicionamos `-g` para gerar símbolos de debug, `-Wall` para exibir todos os avisos de compilação, e um comando entre crases (também chamadas backquotes) que será executado como um comando bash durante a compilação. Este comando chamará o programa `oficina2-config`, que possui predefinições de todas as flags de compilação necessárias para a forma com a qual você compilou a OficinaFramework.
- `CXXLIBS`: Flags de compilação a serem também usadas; mais especificamente, bibliotecas a serem linkadas pelo compilador como dependências do seu jogo. Aqui só reusamos o comando `oficina2-config`, desta vez exprimindo as bibliotecas que são dependências de qualquer jogo feito com a OficinaFramework.
- `CXXOUT`: Flag que vem antes do nome do arquivo executável a ser gerado. Por padrão, é `-o` para GCC e Clang.
- `CXXOBJ`: Flag que define a pré-compilação de um arquivo C/C++ em um arquivo de objeto (`*.o`, `*.obj`). Não usaremos esta flag por enquanto.
- `DEL`: Comando da linha de comando para deleção de um arquivo ou pasta. Aqui um comando bash, para Linux.
- `BIN`: Localização relativa e nome do arquivo binário (programa) a ser gerado na compilação.
- `FILES`: Arquivos de código C++ a serem compilados e transformados no nosso programa, separados por espaço. Por enquanto, usaremos esta definição para compilar vários arquivos de uma vez, mas é aconselhável compilar nosso binário em partes, para que não precisemos recompilar arquivos que não alteramos.
- `.PHONY`: Define os targets que NÃO GERAM arquivos. Aqui definimos `clean`, já que `clean` será apenas um comando para limpar os objetos que geramos (a ser visto mais adiante).

Definimos, também, alguns alvos (targets) de compilação:

- `all`: Alvo de compilação padrão de um Makefile. Tem como função compilar TODOS os outros alvos, ou compilar o projeto inteiro.
- `clean`: Alvo de compilação que funciona apenas como um comando, para limpar os objetos pré-compilados.

Após o texto `all:`, vemos o símbolo `FILES` (colocado entre parênteses e precedido por um cifrão). Isto faz com que esta anotação seja substituída pelo valor que guarda. Este valor, então, será tratado como uma DEPENDÊNCIA daquele target. Ou seja: `all` passará a depender, quando executado, da existência de `src/main.cpp`, e também passará a monitorar alterações neste arquivo. O mesmo valeria para outros arquivos, caso adicionados também a `FILES`.

Abaixo do target `all` e após a indentação por tabulações, temos uma linha usando praticamente apenas símbolos definidos previamente. Estes símbolos, como já explicado, serão substituídos pelo seu valor. Sendo assim, na prática, esta linha será reescrita, durante a compilação, da seguinte forma:

```bash
g++ --std=c++11 src/main.cpp -g -Wall `oficina2-config --cppflags` `oficina2-config --libs` -o bin/MeuJogo
```

Caso você já esteja familiarizado com compilar seus programas C/C++ via terminal do Linux, fica claro que este comando apenas invoca o compilador, dando a ele algumas flags como argumento, bem como dando a ele, também, o nosso arquivo `main.cpp` para compilação. O compilador então gerará o arquivo de saída `MyGame` na pasta `bin`.

A ordem dos argumentos do programa `g++` realmente não possui tanta relevância, EXCETO para as flags `CXXLIBS` e `FILES`. Ao compilar seu jogo feito com a OficinaFramework manualmente ou através de um Makefile, é IMPRESCINDÍVEL que seus arquivos de código-fonte venham ANTES das flags de dependências do projeto; do contrário, você poderá se deparar com erros de dependências indefinidas.

O target `clean` realiza um trabalho parecido, porém removendo arquivos com a extensão `.o` que serão eventualmente gerados e armazenados na pasta `obj`. Por enquanto, não precisamos nos preocupar com isso, uma vez que não geraremos nenhum neste momento.

# Compilando o jogo pela primeira vez

Feito tudo isso, basta agora compilar seu jogo.

Assumindo que você tenha o programa `make` instalado, basta ir até a pasta raiz do jogo, via terminal, e executar o seguinte comando:

```bash
make
```

Algumas linhas de compilação devem ser exibidas e, caso não ocorra nenhum erro inesperado, um arquivo `MeuJogo` deve ser gerado na pasta `bin`.

Você poderá executar este arquivo digitando, a partir da pasta raiz, o comando `bin/MeuJogo` ou `./bin/MeuJogo`, num sistema Linux. Também é possível ir até a pasta do jogo e executá-lo lá dentro, mas prefira executá-lo da pasta rai  para que, futuramente, o jogo consiga localizar seus recursos, que estarão localizados na pasta `res`.

Você verá uma simples tela preta com um tamanho de 1280x720. Por enquanto, não temos absolutamente nada desenhado, o que apenas nos dá a opção de fechar a tela.

Segue abaixo a hierarquia final da nossa pasta:

```bash
MeuJogo
├── bin
│   └── MeuJogo
├── Makefile
├── misc
├── obj
├── res
└── src
    └── main.cpp
```
