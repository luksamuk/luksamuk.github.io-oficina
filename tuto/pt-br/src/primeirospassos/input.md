# Movendo a ofEntity

Agora que temos um objeto renderizado na tela, vamos fazer com que este objeto se mova de acordo com o nosso pressionamento de teclas.

## Entendendo os métodos de entrada e mapeamento

Por razões de padrão e portabilidade, toda a entrada básica de um jogo, ao usar a Oficina, é recuperada a partir de mapeamentos de um controle como o do Xbox 360. Ou seja, a Oficina, por padrão, possui entradas associadas a:

- Botões de controles (A, B, X, Y, LB, RB, LT, RT, LS, RS, Start, Back);
- Analógicos (dois; analógico esquerdo e analógico direito);
- Botões direcionais digitais (Digital Pad Up, Digital Pad Down...);
- Gatilhos (diferente de pressionamentos simples de LT e RT, um valor normalizado indicando a taxa de pressionamento destes gatilhos).

A Oficina suporta um máximo de quatro controles conectados (precisam ser compatíveis com a biblioteca SDL2), e inputs de teclado devem ser diretamente mapeados aos botões citados.
Ao mapear botões de teclado para analógicos, a Oficina oferece ferramentas que relacionam o pressionamento de um botão à movimentação de um certo analógico em uma certa direção.

A engine também possui suporte a posição e cliques do mouse; porém, a posição do ponteiro do mouse sempre será feita a partir do valor absoluto da posição na JANELA, e não no seu viewport! Desta forma, lembre-se de normalizar esta posição, caso um dia seja necessária para você.

Tendo em vista que este mapeamento pode ser problemático para um pequeno projeto sendo feito rapidamente, a Oficina fornece uma função em especial que mapeia automaticamente algumas teclas ao input do Jogador 1. Usaremos, portanto, esta função para mapear nossos controles.

Abra o arquivo `src/main.cpp` e, antes de `ofGameLoop`, digite esta linha de código:

```cpp
ofMapDefaultsP1();
```

Feito isso, poderemos mover nossa entidade, nos próximos tópicos, com as setas do teclado, após utilizarmo-nas apropriadamente. Ou, se assim achar melhor, você pode simplesmente plugar um controle compatível e usar o analógico esquerdo deste controle para interagir da mesma forma.

Para saber um pouco mais sobre as funções disponíveis e sobre o mapeamento, você pode consultar a documentação da Oficina.

## Recebendo métodos de entrada

Agora que mapeamos nossa entrada adequadamente, podemos partir para a forma como devemos lidar com a entrada em si.

A Oficina possui várias funções essenciais para lidar com estes valores. Eis as essenciais, que lidam diretamente com entradas de controladores ou de teclado mapeado:

```cpp
// Analógicos
ofGetLeftStick(jogador);
ofGetRightStick(jogador);
// Botões
ofButtonPress(botao, jogador);
ofButtonTap(botao, jogador);
// Gatilhos
ofGetLeftTrigger(jogador);
ofGetRightTrigger(jogador);
```

Note que, nestas funções, o parâmetro `jogador` é sempre opcional. Caso omitido, a engine assume que você esteja se referindo ao Jogador 1 (`ofPlayerOne`).

Abra o arquivo `src/MinhaEntidade.cpp`.

Logo após os cabeçalhos já incluídos, adicione o cabeçalho de entradas da Oficina:

```cpp
#include <oficina2/input.hpp>
```

Agora, no método `void MinhaEntidade::update`, insira o seguinte código:

```cpp
    glm::vec2 velocidade = ofGetLeftStick();
```

Apesar de não possuir nenhum efeito gráfico, já estamos recuperando a posição do analógico esquerdo do Jogador 1 e salvando-o em um vetor de duas dimensões.
Neste vetor, a coordenada X lida com o plano horizontal, e a Y lida com o plano vertical.

Mover o analógico para a esquerda total e para a direita total significa fazer com que esta coordenada X do vetor se torne -1 e 1, respectivamente; mover o analógico parcialmente também alterará o valor. Um analógico não movido no plano horizontal terá seu valor em X igual a ZERO.
Da mesma forma, mover para cima e para baixo fará com que a coordenada Y assuma valores -1 e 1 respectivamente. Note que, da mesma forma como estabelecemos para nossa projeção anteriormente, o valor se torna maior na direção PARA BAIXO.

Perceba que, no caso do teclado, ele SIMULA este tipo de valor; apertar a seta para a esquerda, por exemplo, fará com que a coordenada X assuma um valor de exatamente -1; apertar a seta para a esquerda fará X assumir 1, e assim por diante. Note, porém, que por padrão não é possível simular, no teclado, algo como um meio-movimento (mover o analógico apenas um pouco para uma direção).

## Calculando a velocidade do movimento

Agora, faremos com que nossa entidade efetivamente SE MOVA na tela.

Primeiramente, é interessante ressaltar que, normalmente, movimentos em jogos são interpolados, já que jogos normalmente possuem uma variação ilimitada na quantidade de quadros processados por segundo. Porém, ao iniciarmos este projeto, configuramos a engine de forma a assegurar que nosso jogo NUNCA ultrapasse a barreira dos 60 quadros por segundo, NO MÁXIMO. Por tanto, podemos operar como se nosso jogo estivesse sempre operando nesta quantidade fixa de quadros por segundo.

Abaixo da linha em que obtemos a posição do nosso analógico, adicione esta linha:

```cpp
    velocidade *= 5.0f;
```

Isto fará com que cada um dos eixos do nosso analógico esquerdo tenha seu valor multiplicado pela taxa de movimento que queremos (5 pixels por quadro ou, numa variação de 60 quadros por segundo, em torno de 300 pixels por segundo).
Perceba que nos valemos de um pequeno artifício da GL Mathematics: estamos multiplicando dois valores, que fazem parte do nosso vetor, com apenas uma linha. Na prática, esta linha faz um trabalho como este:

```cpp
    velocidade.x = velocidade.x * 5.0f;
    velocidade.y = velocidade.y * 5.0f;
```

Agora, portanto, podemos dizer que temos o valor em pixels para o qual sabemos que nosso objeto pode se mover. O sinal desse valor indicará a direção do movimento (por exemplo, se tivermos que nos mover APENAS para a esquerda, teremos um valor como {x = -5, y = 0}).

## Transformando a entidade

Por fim, agora, só falta incorporar esta variação de posição do objeto ao próprio objeto.

Mais cedo, utilizamos o método `translate` para posicionar nosso objeto na tela. Este método, em conjunto com `rotate` e `scale`, é herdado também da casse `ofEntity`.
Estes três métodos especiais fazem com que seja possível alterar aspectos da nossa classe ou, mais especificamente: posição, ângulo de rotação e escala.

Vá ao fim do mesmo método `void MinhaEntidade::update`, e adicione esta linha, depois do cálculo da velocidade:

```cpp
    translate(glm::vec3(velocidade, 0.0f));
```

O que estamos fazendo é uma translação direta na matriz de posição do nosso objeto. O argumento `glm::vec3(velocidade, 0.0f)` cria um vetor tridimensional a partir dos valores X e Y de velocidade e, no que tange ao valor Z (que está faltando em `velocidade`), usamos 0.0f, já que vamos nos manter em duas dimensões.

Compile e execute seu aplicativo. Você agora poderá mover o quadrado usando as setas do teclado ou, caso ache melhor, conectando a qualquer momento um controle compatível e usando o analógico esquerdo do controle.

Eis o método `update` da entidade, após o experimento:

```cpp
void MinhaEntidade::update(float dt)
{
    glm::vec2 velocidade = ofGetLeftStick();
    velocidade *= 5.0f;
    translate(glm::vec3(velocidade, 0.0f));
}
```
