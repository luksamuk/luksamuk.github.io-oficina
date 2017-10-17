# Hierarquia
Comece criando um diretório para seu jogo. Por exemplo, criaremos um diretório chamado "MeuJogo".
Dentro deste diretório, vamos criar alguns subdiretórios chamadas "bin", "misc", "obj", "res" e "src".

A pasta ficará com a seguinte hierarquia:

```bash
MeuJogo
├── bin
├── misc
├── obj
├── res
└── src
```

Eis uma explicação para o uso de cada subdiretório:

- `bin`: Diretório onde ficarão as compilações do nosso jogo;
- `misc`: Diretório onde ficarão todos os arquivos extras do jogo, que não serão incluídos na distribuição dele;
- `obj`: Diretório onde ficarão os objetos pré-compilados do jogo. No primeiro momento, não lidaremos com estes objetos;
- `res`: Diretório onde ficarão os resources do jogo, ou seja, os arquivos que serão usados pelo jogo e incluidos na distribuição do mesmo. Isso inclui scripts, texturas, etc;
- `src`: Diretório onde ficará o código-fonte do jogo a ser compilado.

Lembre-se de que esta hierarquia de diretórios é apenas uma convenção, que aqui usaremos para organizar melhor nosso projeto.
