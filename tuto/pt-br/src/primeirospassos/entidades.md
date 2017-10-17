# Criando uma entidade através da classe ofEntity

Após uma noção inicial da hierarquia de um `ofCanvas`/cena, poderemos, então, dar prosseguimento à definição de um objeto, entidade, ou, como definido na Oficina, `ofEntity`.

Entidades funcionam de forma similar a uma cena, no sentido de que também herdam certos métodos e propriedades de uma classe abstrata. Porém, entidades possuem algumas coisas específicas, como sua própria matriz Model (ou melhor dizendo, as matrizes componentes do objeto); esta matriz fica responsável por armazenar posição, rotação e escala do objeto em questão.

Vamos ao código e, logo depois, analisaremos o que precisamos.

Você vai precisar criar mais dois arquivos: um cabeçalho e um arquivo de código, ambos na pasta `src`. Vá em frente e crie `src/MinhaEntidade.hpp` e `src/MinhaEntidade.cpp`, colocando, dentro de cada um desses arquivos, conforme mostrado abaixo:

```cpp
// MinhaEntidade.hpp
#pragma once

#include <oficina2/entity.hpp>
using namespace oficina;

class MinhaEntidade : public ofEntity
{
private:
public:
    void init();
    void load();
    void unload();
    void update(float dt);
    void draw(glm::mat4 mvp);
};
```

```cpp
// MinhaEntidade.cpp
#include "MinhaEntidade.hpp"

void MinhaEntidade::init()
{
}

void MinhaEntidade::load()
{
}

void MinhaEntidade::unload()
{
}

void MinhaEntidade::update(float dt)
{
}

void MinhaEntidade::draw(glm::mat4 mvp)
{
}
```

Além disso, você precisará, como feito com nossa cena na subseção anterior, adicionar uma referência ao cabeçalho em `src/MinhaCena.hpp` e uma referência ao arquivo de código em si no nosso `Makefile`.

Em `src/MinhaCena.hpp`, após a inclusão do cabeçalho `oficina2/render.hpp`, digite:

```cpp
#include "MinhaEntidade.hpp"
```

Já em `Makefile`, substitua o macro `FILES` de forma que ele mostre o seguinte:

```Makefile
FILES    = src/main.cpp src/MinhaCena.cpp src/MinhaEntidade.cpp
```

Veja que, na realidade, uma entidade possui uma estrutura muito parecida com a cena, à exceção dos detalhes apresentados, que estão relacionados a transformações na entidade (detalhes com os quais ainda não tivemos contato direto).
A única diferença destacável é o fato de que, em um método `draw` de uma entidade, recebemos, como argumento, a matriz `mvp` da nossa CENA. Veremos como lidar com isso no próximo tópico.

Feito isso, você pode testar se ocorreu tudo bem, compilando sua aplicação mais uma vez.

# Instanciando manualmente uma entidade

Após digitar o código, absolutamente tudo está apropriadamente definido, exceto pelo fato de que nós ainda não possuimos NENHUMA instância da entidade, que acabamos de criar, em nenhuma parte do nosso jogo.

Por enquanto, vamos consertar isso instanciando manualmente UMA intidade do nosso jogo.

Vá ao arquivo `src/MinhaCena.hpp`.  Abaixo da instância da nossa fonte, insira o seguinte:

```cpp
MinhaEntidade entidade;
```

Note que não estamos criando nossa entidade dinamicamente, mas isto também não significa que ela está inicializada, muito menos incluída na lógica da cena!
Para tanto, vá agora ao arquivo `src/MinhaCena.cpp`. Lá, nós vamos nos assegurar de que:

- A entidade seja inicializada (método `init`);
- A entidade tenha seu conteúdo essencial carregado (método `load`);
- A entidade tenha seu conteúdo essencial descarregado ao fim da cena (método `unload`);
- A entidade tenha sua lógica atualizada quadro-a-quadro (método `update`);
- A entidade seja desenhada quadro-a-quadro (método `draw`).

Você pode já ter percebido onde queremos chegar.
Adicione as linhas de código a seguir nos respectivos métodos referidos:

```cpp
// em "void MinhaCena::init", após a declaração da mvp:
entidade.init();

// em "void MinhaCena::load", após o carregamento da fonte de texto:
entidade.load();

// em "void MinhaCena::unload", após o descarregamento da fonte de texto:
entidade.unload();

// em "void MinhaCena::update":
entidade.update(dt);

// em "void MinhaCena::draw", após a escrita de texto na tela:
entidade.draw(mvp);
```

Você pode compilar mais uma vez e verificar se o código corre sem erros; Porém, você pode verificar se seu código esta sendo executado sem problemas de outra forma, como veremos a seguir.
