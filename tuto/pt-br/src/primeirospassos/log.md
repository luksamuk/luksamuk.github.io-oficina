# Isolando bugs e colhendo informações: A função ofLog

Para verificar melhor o funcionamento do programa, é bom que tenhamos um sistema de log, que imprima texto na tela. Para isso, temos a função `ofLog`.

Na realidade, `ofLog` é uma função muito parecida com `sprintf` e `fprintf`. Porém, `ofLog` é dinâmica no sentido de que permite configuração de níveis de gravidade do texto de log a ser impresso, bem como redirecionamento: você pode escolher entre imprimir no console, imprimir num arquivo de texto, ou não imprimir os logs.

Esta função pode ser usada da seguinte forma:

```cpp
ofLog(nivel, formato, ...);
```

- `nivel`: Nível de gravidade do log. Os níveis suportados são:
	- `ofLogCrit`: Erro crítico, possivelmente ocasionando uma quebra no funcionamento do programa. Este tipo de erro, quando o log é suprimido, exibirá uma caixa de texto comuma mensagem de erro na tela;
	- `ofLogErr`: Mensagem de erro;
	- `ofLogWarn`: Mensagem de aviso;
	- `ofLogInfo`: Mensagem de informação;
	- `ofLogNone`: Mensagem sem nível de propósito simples.
- `formato`: Formato da string de saída, da mesma forma que `printf`. Recomenda-se encerrar a string com o caractere `\n`;
- `...`: Elipses; variáveis que farão parte do formato, da mesma forma que em `printf`.

Além disso, você ainda pode usar as seguintes funções para definir propriedades do seu log:

```cpp
ofLogSetLevel(nivel);
```

Onde `nivel` é o seu nível mínimo de log. Qualquer nível ABAIXO deste nível de log será automaticamente descartado da saída.


```cpp
ofLogUseFile(nome_do_arquivo);
ofLogUseConsole();
ofLogDisable();
```

Estas três funções estabelecem onde o log será impresso.

- `ofLogUseFile` determina que o log seja impresso num arquivo à parte, caso `nome_do_arquivo` seja um arquivo válido ou possa ser criado;
- `ofLogUseConsole` devolve o log ao console onde o jogo está sendo executado, caso o log esteja sendo feito em um arquivo ou esteja desabilitado;
- `ofLogDisable` suprime toda e qualquer tentativa de impressão de log, a não ser janelas de mensagens de erro crítico.

Abra o arquivo `src/MinhaEntidade.cpp`.
Abaixo da inclusão do arquivo `MinhaEntidade.hpp`, escreva a seguinte linha:

```cpp
#include <oficina2/io.hpp>
```

Agora, adicione as seguintes linhas aos seus respectivos métodos:

```cpp
// em "void MinhaEntidade::init":
ofLog(ofLogWarn, "Entidade inicializada!\n");

// em "void MinhaEntidade::load":
ofLog(ofLogWarn, "Entidade carregada!\n");

// em "void MinhaEntidade::unload":
ofLog(ofLogWarn, "Entidade descarregada!\n");
```

Após inicializar e imediatamente fechar a aplicação, você verá uma mensagem de saída, no seu console, similar a este log:

```
INFO: Powered by OficinaFramework v2.0.11
INFO: ofInit: Creating generic display and context
INFO: ofInit: Pushing gameargs
INFO: ofInit: Opening display and context
INFO: ofDisplay.parseArgs: Window size set to (800, 600)
INFO: ofDisplay.setSwapInterval: Set a cap to 60.00FPS.
INFO: ofContext.open: 
  OpenGL v3.3.0 NVIDIA 378.13
  GLSL Shader Model v3.30 NVIDIA via Cg compiler
  GL Extension Wrangler v2.0.0
  Renderer: GeForce GT 525M/PCIe/SSE2
  Vendor: NVIDIA Corporation
  ARB Debug Log: Yes
INFO: ofInit: Starting canvas manager
INFO: ofLoadDefaultFont: Uploaded GohuFont (Hardcoded) to VRAM
INFO: ofShader.compile: Compilation successful
INFO: ofShader.compile: Compilation successful
INFO: ofInit: Starting global REPL interpreter
INFO: Initializing Scheme REPL...
INFO: ofGameLoop
WARN: Entidade inicializada!
INFO: ofLoadDefaultFont: Uploaded Fixedsys Excelsior (Hardcoded) to VRAM
WARN: Entidade carregada!
INFO: ofSoftStop
INFO: ofQuit: Unloading canvases
INFO: ofTexturePool.unload: Deleted Fixedsys Excelsior (Hardcoded) from VRAM
WARN: Entidade descarregada!
INFO: ofTexturePool.unload: Deleted GohuFont (Hardcoded) from VRAM
INFO: ofQuit: Unloading REPL interpreter
INFO: Uninitializing Scheme REPL...
INFO: ofQuit: Unloading texture pool
INFO: ofTexturePool.clear: Clearing pool
INFO: ofTexturePool.clear: Cleared
INFO: ofQuit: Closing context and display
```

Observe que nossas mensagens de log na entidade receberam um prefixo `WARN: ` (provavelmente na cor amarela, caso seu console suporte caracteres coloridos), em conformidade com o nível de log que demos a elas (`ofLogWarn`).

Por último, você pode, também, imprimir texto colorido, caso seu console suporte isso. Através do truque de concatenação de strings de C++, existem alguns macros predefinidos no cabeçalho `oficina2/io.hpp` que auxiliam nesta façanha.
Por exemplo, um comando como

```cpp
ofLog(ofLogInfo, OFLOG_GRN "Hello, " OFLOG_RESET "world!\n");
```

imprimirá a string "Hello, " em letras verdes, enquanto "world!\n" será impresso na cor normal de texto do console.
Na verdade, o que ocorre é uma concatenação automática de literais de string, feitas durante a compilação do jogo.

Você pode consultar estes macros nas documentações. Apenas lembre-se de usar a macro `OFLOG_RESET` ao fim da sua string!
