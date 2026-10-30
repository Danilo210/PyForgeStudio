# PyForge Studio — IDE Python no navegador

**Crédito: Danilo Monteiro da Silva**  
Versão 1.0 — projeto de IDE Python para publicação como site estático.

## O que funciona

- Editor de arquivos `.py`, `.txt`, `.json`, `.csv`, `.sql`, `.html`, `.md` e outros formatos texto com realce de sintaxe Python, números de linha, identação automática, parênteses automáticos, Ctrl+Espaço para sugestões, Ctrl+F para localizar e substituir, Ctrl+Enter para executar.
- Exploração de arquivos e pastas; criar, renomear, excluir e baixar; abas; imagens importadas exibidas em painel; arquivos binários presentes no sistema virtual de arquivos Python.
- Salvar projeto automaticamente no `localStorage` do navegador e exportar/importar projetos em ZIP ou backup JSON. O JSZip necessário é **incluído no próprio `index.html`**.
- Executar Python em um Web Worker via **Pyodide 0.29.5**. O interpretador não é carregado na abertura da página: é transferido no primeiro clique em **Executar** ou **Instalar bibliotecas**.
- Gerenciar bibliotecas via Pyodide/micropip, com sugestões de `numpy`, `pandas`, `matplotlib`, `sympy`, `pillow`, entre outras. As dependências são registradas em `requirements.txt` e incluídas na exportação.
- Importar imagens pequenas, CSV, JSON, e arquivos auxiliares. Arquivos produzidos pelo programa no diretório do projeto (por exemplo `dados/resultado.txt` ou `dados/grafico.png`) são sincronizados para o explorador ao final da execução.
- Terminal com stdout/stderr, interrupção do código (encerra o Web Worker), linhas de `input()` configuradas antes da execução, temas escuro/claro e visual responsivo.

## Hospedar gratuitamente no Render

1. Descompacte `PyForge_Studio.zip`.
2. Envie **somente `index.html`** para a raiz do repositório GitHub (a documentação é opcional).
3. No Render, clique em **New → Static Site**.
4. Conecte o repositório, branch `main`.
5. Em **Build Command** use `echo "Ready"`; em **Publish Directory** use `.`.
6. Clique em Deploy.

Para executar localmente, abra `index.html` ou rode um servidor HTTP, por exemplo `python -m http.server 8080` no diretório do projeto. Servir via HTTP/HTTPS é recomendado para o worker Python.

## Bibliotecas — instruções

1. Escreva ou abra código Python em um arquivo `.py`.
2. No painel **Gerenciador de pacotes**, escolha uma biblioteca pronta ou escreva o nome dela e clique `+`.
3. Clique em **Instalar bibliotecas**, ou clique diretamente em **Executar** (as bibliotecas selecionadas serão instaladas antes da execução).
4. Após isso, use `import numpy`, `import pandas`, etc. no Python.

**Exemplo com biblioteca** em `exemplos/grafico.py`, incluído no projeto de demonstração. O exemplo cria `dados/grafico.png` a partir de `numpy` e `matplotlib` e o disponibiliza para visualização/baixar no explorador.

**Suporte real de bibliotecas:** o Pyodide pode instalar pacotes preparados para WebAssembly e bibliotecas puras de Python disponibilizadas como wheels; nem todo pacote do `pip` é compatível. Pacotes que exigem serviços/recursos do sistema operacional, como `tkinter`, `pywin32`, drivers externos de bancos de dados e bibliotecas com extensões nativas indisponíveis no Pyodide, não são suportados. Requisições HTTP do navegador são limitadas por CORS, inclusive via `requests`.

Pacotes e o próprio interpretador precisam de conexão com a internet na primeira utilização e são carregados pela CDN. O browser pode reutilizar conteúdos em cache, mas não há garantia de execução offline. Sem internet, o editor e a exportação ZIP continuam funcionando, mas não a instalação de bibliotecas ou o primeiro carregamento Python.

## Limites e segurança

- Site estático, **sem servidor Python**. A execução roda no navegador do usuário, em um Worker WebAssembly, não em um terminal remoto ou máquina Windows.
- A opção **Parar** interrompe o Worker: o estado do interpretador e as bibliotecas instaladas nessa sessão serão recarregados no próximo uso. Os arquivos de projeto salvos no navegador permanecem.
- Os dados são salvos apenas no navegador onde você usa a IDE. Use regularmente **Exportar ZIP** para ter cópias e mover projetos entre computadores.
- A importação individual está limitada a 2,5 MB por arquivo. A sincronização de arquivos criados pelo programa limita cada arquivo a 1,5 MB e a sincronização total a 5 MB, para evitar bloquear o navegador. O armazenamento local disponível depende do navegador.
- Programas Python executados no Pyodide podem realizar ações de rede permitidas pelo navegador e consumir recursos da máquina. Não execute códigos desconhecidos ou instale pacotes de fontes não confiáveis.
- O `stdin` é informado antes da execução (use `|` para separar respostas a vários `input()`). Não é um terminal TTY totalmente interativo.
- O código JavaScript e o layout foram verificados em navegador local; a descarga do interpretador e o funcionamento de cada biblioteca externa dependem da CDN e de compatibilidade com Pyodide e **não foram validados em uma implantação Render real**.

## Arquivos entregues

- `index.html` — interface e lógica da IDE, com JSZip incorporado, pronto para publicação.
- `README.md` — este guia.
- `LICENCAS_TERCEIROS.md` — licença JSZip.
- `preview-desktop.png`, `preview-mobile.png` — capturas da interface (não são necessárias no Render).

**© Danilo Monteiro da Silva — PyForge Studio**
