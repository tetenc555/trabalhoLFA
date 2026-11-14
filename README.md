# Analisador Léxico — Linguagens Formais e Autômatos

## Sobre o projeto

Este projeto é um protótipo inicial de um framework para análise léxica, desenvolvido como trabalho acadêmico para a disciplina de **Linguagens Formais e Autômatos**. A proposta é estudar, na prática, como um Autômato Finito Determinístico (AFD) pode reconhecer sequências de caracteres e identificar tokens. O projeto também será aplicado à web. 

## Tarefas realizadas

- [x] Criada a classe principal em Java para iniciar a análise léxica.
- [x] Carregada a definição do AFD a partir de um arquivo XML.
- [x] Implementada a leitura do texto de entrada caractere por caractere.
- [x] Implementado o acompanhamento das transições do AFD para formar tokens.
- [x] Implementada a identificação de estados finais para reconhecer tokens.
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
- [ ] Melhorar a apresentação das mensagens de erro e dos resultados da análise.
- [ ] **Classificação dos Tokens (Par `<Tipo, Lexema>`):** Atualmente, o método `lexico()` retorna apenas uma `String` com o texto lido (o *lexema*). Um analisador léxico precisa retornar um objeto `Token` contendo a sua categoria (ex: `IDENTIFICADOR`, `NUMERO`, `OPERADOR`) associada ao valor lido, geralmente mapeada a partir do estado final atingido no AFD.
- [ ] **Diferenciação de Palavras Reservadas:** Falta uma verificação (geralmente via tabela hash ou lista) para checar se um lexema reconhecido como identificador é, na verdade, uma palavra-chave da linguagem (como `if`, `while`, `int`).
- [ ] **Regra da Maior Correspondência (*Maximal Munch*) e Retrocesso (*Unget/Unread*):** No código atual, assim que o autômato atinge um estado final (`if (a.getEstadosFinais().pertence(corrente))`), ele para imediatamente. Se o código tiver a variável `contador`, ele vai parar na primeira letra (`c`) se o estado for final. Ele precisa continuar lendo até não haver mais transição válida e **devolver o último caractere lido** ao *buffer* (usando `PushbackReader` ou `mark()`/`reset()` do `BufferedReader`).
- [ ] **Ignorar Espaços em Branco e Comentários:** O código atual converte quebras de linha (`charLido == 10`) em espaços `' '` e os envia para o AFD. Espaços, tabulações (`\t`), retornos de carro (`\r`) e comentários devem ser consumidos e descartados antes ou durante a formação do token, sem gerar retorno para o próximo estágio.
- [ ] **Rastreamento de Linha e Coluna:** Não há contadores para `linha` e `coluna`. Sem isso, quando ocorrer um `"erro"`, o compilador não consegue informar ao programador em qual linha do arquivo o erro léxico aconteceu.
- [ ] **Tratamento de Fim de Arquivo (EOF) no meio de um token:** No `while (p != null)`, se o arquivo acabar enquanto o autômato está no meio de um token válido (mas ainda não atingiu um estado final), o código simplesmente retorna `"fim"` em vez de acusar um erro léxico (ex: uma string ou comentário aberto que nunca foi fechado antes do EOF).
- [ ] **Integração com Tabela de Símbolos:** Falta inserir ou consultar identificadores e literais em uma Tabela de Símbolos para otimizar o uso de memória nas fases seguintes da compilação.
- [ ] Integrar o analisador a uma aplicação web, com entrada de texto e exibição dos tokens e erros.
