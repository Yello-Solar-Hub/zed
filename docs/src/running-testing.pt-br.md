---
título: Executar e testar código no Zed
descrição: Execute e teste códigos no Zed utilizando fluxos de trabalho com o Terminal, Tarefas, REPL e depurador, sem sair do editor.
---

# Execução e testes

Use esta seção para executar e testar código no Zed e, em seguida, depurar problemas sem
saindo do editor.

## O que há aqui

- **[Terminal](./terminal.md)**: O emulador de terminal integrado ao Zed. Abra vários terminais, personalize seu shell e integre-o ao editor. As tarefas e os comandos são executados aqui.

- **[Tarefas](./tasks.md)**: Defina e execute comandos de shell com acesso ao contexto do editor, como o arquivo atual, a seleção ou um símbolo. Use as tarefas para compilar, verificar a conformidade do código, rodar scripts ou executar qualquer fluxo de trabalho repetível.

- **[Depurador](./debugger.md)**: Defina pontos de interrupção, execute o código passo a passo e inspecione variáveis usando o depurador integrado do Zed. Funciona com C, C++, Go, JavaScript, Python, Rust, TypeScript e outras linguagens por meio do Protocolo de Adaptador de Depuração (DAP).

- **[REPL](./repl.md)**: Execute código de forma interativa usando kernels do Jupyter. Execute seleções ou células e veja os resultados diretamente na página — útil para Python, TypeScript (Deno), R, Julia e outras linguagens compatíveis.

## Introdução rápida

**Abrir um terminal**: Pressione ``Ctrl+``` para alternar entre o painel do terminal ou `Ctrl+~` para abrir um novo terminal.

**Executar um comando**: Pressione `Cmd+Shift+R` (macOS) ou `Ctrl+Shift+R` (Linux/Windows) para abrir o seletor de tarefas e, em seguida, digite qualquer comando do shell.

**Iniciar a depuração**: Pressione `Cmd+Shift+D` (macOS) ou `Ctrl+Shift+D` (Linux/Windows) para abrir o painel de depuração e selecionar uma configuração.

**Executar código de forma interativa**: Em um arquivo Python ou TypeScript, selecione parte do código e pressione `Ctrl+Shift+Enter` para executá-lo em uma sessão REPL.
