# Analisador Léxico — Linguagens Formais e Autômatos

## Sobre o projeto

Este projeto é um protótipo inicial de um framework para análise léxica, desenvolvido como trabalho acadêmico para a disciplina de **Linguagens Formais e Autômatos**. A proposta é estudar, na prática, como um Autômato Finito Determinístico (AFD) pode reconhecer sequências de caracteres e identificar tokens. O projeto também será aplicado à web.

## Tarefas realizadas

- [x] Criada a classe principal em Java para iniciar a análise léxica.
- [x] Carregada a definição do AFD a partir de um arquivo XML.
- [x] Implementada a leitura do texto de entrada caractere por caractere.
- [x] Implementado o acompanhamento das transições do AFD para formar tokens.
- [x] Implementada a identificação de estados finais para reconhecer tokens (parada ao atingir o estado final, como o estado 3 do PDF).
- [x] Implementado o tratamento básico de fim de arquivo e de erro léxico.
- [x] Criados arquivos de exemplo para a definição do AFD e para o texto de entrada.

## Organização atual

| Arquivo | Finalidade |
| --- | --- |
| `Principal.java` | Inicializa o AFD, lê a entrada e coordena a análise léxica. |
| `AFD.XML` | Define o alfabeto, os estados, o estado inicial, os estados finais e as transições do AFD de exemplo. |
| `arquivo.txt` | Texto de exemplo usado como entrada para o analisador. |

## Próximas etapas

- [ ] Organizar o projeto e seus caminhos de entrada para facilitar a execução.
- [ ] **Simulação do AFD (Aula 04):** implementar o algoritmo de simulação (`s := s0; enquanto c != eof: s := movimento(s,c)`) que responde "sim"/"não" para uma palavra, tratando `movimento` indefinido como estado inválido.
- [ ] **Classes de símbolos no AFD:** permitir no `AFD.XML` (ou na leitura) as classes `letra` (a–z), `dígito` (0–9), `del` (branco, tab, quebra de linha) e `outro = A − {...}`, evitando listar um símbolo por transição.
- [ ] **AFDs do analisador léxico:** definir no `AFD.XML` os três autômatos do PDF:
  - delimitadores: `del` `del*` `outro*`
  - identificadores: `letra` `(letra | dígito)*` `outro*`
  - inteiros sem ssinal: `dígito` `dígito*` `outro*`
- [ ] **Estado final com retrocesso (`*`):** marcar no AFD os estados finais "com asterisco" (o estado 3 do PDF). Ao atingí-los, remover o último caractere do lexema e **devolvê-lo à entrada** (`PushbackReader` ou `mark()`/`reset()`), para que o próximo token comece nele. Isso garante o token de **maior comprimento possível**.
- [ ] **Classificação dos tokens (par `<Tipo, Lexema>`):** `lexico()` hoje retorna só uma `String`. Criar a classe `Token` com o tipo (`DELIMITADOR`, `IDENTIFICADOR`, `NUMERO`) e o lexema, definido pelo estado final atingido.
- [ ] **Delimitadores como token:** tratar branco, tab (`\t`), retorno de carro (`\r`) e quebra de linha (`\n`) como a classe `del` (hoje só `\n` vira `' '`). O token de delimitador é reconhecido pelo AFD e descartado pelo driver, sem aparecer na saída.
- [ ] **Tratamento do EOF como símbolo `outro`:** ao chegar ao fim do arquivo, enviar um símbolo `eof` ao AFD. Assim, um token no final do texto sem delimitador depois (ex.: `abc<eof>`) é reconhecido. Se o EOF ocorrer em estado não final, reportar erro léxico em vez de retornar `"fim"`.
- [ ] **Revisar o início de `lexico()`:** o teste "estado inicial é final → retorna `fim`" não corresponde ao PDF (o estado inicial não é final nesses AFDs). O fim deve ser detectado pela leitura de EOF.
- [ ] **Mensagens de erro e rastreamento:** contar `linha` e `coluna` durante a leitura e exibir no erro léxico o caractere inválido e sua posição. Melhorar também a apresentação dos resultados.
- [ ] **Testes com os exemplos do PDF:** `tmp`, `a1`, `x0` (identificadores); `10`, `00001`, `513` (inteiros); sequências com vários delimitadores seguidos; entrada que termina sem delimitador; símbolo inválido.
- [ ] Integrar o analisador a uma aplicação web, com entrada de texto e exibição dos tokens e erros.
