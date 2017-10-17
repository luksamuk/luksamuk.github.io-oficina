# Criando uma cena (ofCanvas)
Antes de realizar quaisquer operações novas no nosso jogo, é necessário que haja algo que possa ser associado como uma "tela de pintura" na sua janela. Neste caso, a OficinaFramework possui uma classe de objetos que disponibiliza uma abstração neste sentido.
A classe `oficina::ofCanvas` disponibiliza controles e métodos pré-instanciados para que possamos criar nossas próprias "telas de pintura". Esta abstração, em conjunto com o objeto estático `oficina::ofCanvasManager` tornará possível a coexistência de uma ou mais cenas na tela, sendo renderizadas e tendo sua lógica atualizada ao mesmo tempo.

## Declarando e compilando a cena

Para tanto, crie dois arquivos: `src/MinhaCena.hpp` e `src/MinhaCena.cpp`.

Em `src/MinhaCena.hpp`, insira o código como mostrado abaixo:

```cpp
// MinhaCena.hpp
#pragma once

#include <oficina2/canvas.hpp>
using namespace oficina;

class MinhaCena : public ofCanvas
{
private:
    glm::mat4 mvp;
public:
    void init();
    void load();
    void unload();
    void update(float dt);
    void draw();
};
```

O código consiste na declaração de uma classe, que recebe como herança os métodos e campos internos da classe `oficina::ofCanvas`. Os métodos herdados são:

- `init`: Método para inicialização de lógica em geral, sendo executado logo antes do método `load`;
- `load`: Método especial para carregamento de recursos do jogo (texturas, scripts, etc), executado após o método `init`;
- `unload`: Método especial para descarregamento de recursos do jogo, executado durante a remoção/deleção do `ofCanvas`;
- `update`: Método para execução de lógica em geral, sendo chamado uma vez a cada quadro da aplicação. O parâmetro `dt` especifica, em SEGUNDOS, a diferença de tempo entre o quadro renderizado anteriormente e o quadro renderizado atualmente. Este número pode variar livremente ou ser fixo, de acordo com o especificado no controle de quadros do jogo. Não usaremos este parâmetro neste tutorial, uma vez que sabemos que nosso jogo executará sob um limite de 60 quadros por segundo, mas é um parâmetro muito útil ao realizar interpolações na física de um jogo.
- `draw`: Método para execução da lógica de desenho da cena.

Em adição, também declaramos um campo privado chamado `mvp`, uma matriz 4x4. O uso do prefixo `glm::` está relacionado à biblioteca onde ela é implementada, a GL Mathematics, da qual a Oficina faz extenso uso. Esta é uma sigla para a expressão "Model-View-Projection", relacionada à multiplicação destas três importantes matrizes no mundo da computação gráfica. Trata-se da matriz que vai especificar a forma como a cena será renderizada na tela: a câmera e o tamanho da resolução interna, bem como a "posição" da mesma. Na realidade, especificaremos aqui apenas as matrizes View e Projection, já que a matriz Model será calculada para cada entidade que colocarmos na tela.

Este não é um tutorial introdutório de computação gráfica ou de geometria analítica, mas é importante dizer que a ordem de multiplicação entre uma matriz e outra é extremamente importante. Para tanto, devemos fixar que, ao gerar a matriz `mvp` em qualquer linguagem similar a C++, devemos nos certificar de que multiplicamos as matrizes nesta ordem:

```
mvp = Projection * View * Model
```

Agora, digite este código no arquivo `src/MinhaCena.cpp`:

```cpp
// MinhaCena.cpp
#include "MinhaCena.hpp"

void MinhaCena::init()
{
}

void MinhaCena::load()
{
}

void MinhaCena::unload()
{
}

void MinhaCena::update(float dt)
{
}

void MinhaCena::draw()
{
}
```

Como você pode ver, o código acima não passa de definições vazias para a classe que declaramos.

Agora, precisamos nos certificar de que o arquivo `MinhaCena.cpp` seja compilado. para tanto, abra o arquivo `Makefile`, e altere o macro `FILES` de forma que fique assim:

```Makefile
FILES    = src/main.cpp src/MinhaCena.cpp
```

Agora, daremos à engine uma instância da nossa cena, de forma que a engine gerencie-a e atualize-a como necessário.
Abra o arquivo `src/main.cpp` e, entre as chamadas das funções `ofInit` e `ofGameLoop`, insira o seguinte:

```cpp
ofCanvasManager::add(new MinhaCena, 0, "Minha Cena");
```

Esta chamada adicionará uma nova instância da nossa cena ao gerenciador de cenas interno da engine.
O primeiro argumento é efetivamente um PONTEIRO para nossa cena, que obrigatoriamente deve ser um objeto que herdou os métodos de `ofCanvas`. O segundo argumento é a profundidade da cena, ou a ordem com a qual ela deve aparecer em relação às outras cenas; como só possuimos uma cena atualmente, esta ordem é irrelevante. O terceiro argumento é um nome opcional para a cena, que manteremos para fins didáticos.


Por último, precisamos nos certificar de que o arquivo `src/main.cpp` conheça a declaração de classe da nossa cena. Ainda em `src/main.cpp`, logo após o cabeçalho da OficinaFramework, insira a seguinte linha:

```cpp
#include "MinhaCena.hpp"
```

O código final de `src/main.cpp` deverá estar assim:

```cpp
#include <oficina2/oficina.hpp>
using namespace oficina;

#include "MinhaCena.hpp"

int main(int argc, char** argv)
{
    ofInit({
        "wname=Meu Jogo",
        "datad=MeuJogo",
        "winsz=800x600",
        "frmrt=60c"
    });
    ofCanvasManager::add(new MinhaCena, 0, "Minha Cena");
    ofGameLoop();
    ofQuit();
    return 0;
}
```
