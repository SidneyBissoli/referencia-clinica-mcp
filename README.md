# referencia-clinica-mcp

**Produto planejado. Nada foi construído ainda.** Este repositório existe para descrever
o produto e servir de referência à consulta de enquadramento sanitário feita à ANVISA
em outubro de 2026. O código só começa depois da resposta.

## O que será

Um servidor MCP (*Model Context Protocol*), gratuito e de código aberto, que recupera e
exibe, com fonte, versão e data, trechos de documentos oficiais públicos sobre
medicamentos no Brasil. É usado por meio de assistentes de IA de uso geral (por exemplo,
Claude ou ChatGPT), escolhidos e operados pelo próprio usuário.

Equivale a consultar a bula, a lista da CMED ou o PCDT diretamente na fonte. O público
é o profissional de saúde.

## Funções previstas

1. **A partir de um nome comercial ou de um princípio ativo:** o princípio ativo (DCB),
   a classe terapêutica, os produtos registrados na ANVISA, o preço máximo regulado
   pela CMED e trechos literais da bula.
2. **A partir de um código CID-10:** os Protocolos Clínicos e Diretrizes Terapêuticas
   (PCDT) oficiais que o citam, com o trecho, a portaria e o link do PDF.

Toda resposta traz a fonte, a versão ou data da publicação e a URL. Quando a fonte não
traz a informação (por exemplo, medicamento sem bula disponível), o software informa a
ausência e não a preenche.

## O que não faz, por desenho

- não recebe dados de pacientes (idade, peso, função renal, diagnóstico, comorbidades,
  medicamentos em uso de um paciente identificado);
- não calcula nem ajusta doses;
- não avalia, classifica, pontua nem ranqueia interações, riscos ou alternativas;
- não recomenda, sugere nem escolhe medicamento, exame ou conduta;
- não gera alertas;
- não armazena o conteúdo das consultas.

Não há algoritmo de pontuação de risco, árvore de decisão, heurística clínica nem modelo
de aprendizado de máquina. O único ordenamento é a relevância textual entre o nome
pesquisado e o nome do documento. O software não usa IA; é acessado por assistentes de
IA, que não são parte do produto.

## Fontes

- Denominação Comum Brasileira (DCB), Instrução Normativa ANVISA nº 462/2026;
- dados abertos de registro de medicamentos da ANVISA;
- lista de preços da Câmara de Regulação do Mercado de Medicamentos (CMED);
- índice de Protocolos Clínicos e Diretrizes Terapêuticas (PCDT) da Conitec;
- bulas publicadas pela FDA dos EUA (openFDA), enquanto o bulário eletrônico da ANVISA
  não oferece acesso programático.

## Como funciona

- **Entrada:** nome de medicamento, código ATC ou código CID-10.
- **Processamento:** normalização do nome e busca em tabelas pré-computadas a partir das
  fontes oficiais (DCB → número CAS → UNII → RxNorm, para localizar a bula), seguidas de
  consultas às APIs públicas das fontes.
- **Saída:** trechos literais dos documentos, com fonte, data e URL, sem alteração do
  conteúdo.
- **Execução:** servidor na nuvem (Cloudflare Workers), acessado por HTTPS, ou
  localmente no computador do usuário. Registra apenas métricas agregadas de uso (nome
  da ferramenta chamada, sucesso ou erro, duração).

## Exemplo de uso (planejado)

Pergunta do profissional ao assistente de IA: *"Qual o preço máximo da metformina 850 mg
e o que a bula diz sobre uso na gravidez?"*

O assistente chama o servidor, que devolve, por exemplo:

```
princípio ativo: cloridrato de metformina (nome DCB)
  fonte: DCB, IN ANVISA nº 462/2026, https://bibliotecadigital.anvisa.gov.br/...
classe terapêutica: biguanidas (ATC A10BA02)
produtos registrados na ANVISA: N apresentações de 850 mg (lista completa na resposta)
  fonte: dados abertos de registro, ANVISA, extraído em 2026-10-10
preço máximo ao consumidor (PMC 18%): R$ 00,00 a R$ 00,00 conforme a apresentação
  fonte: lista CMED de 23/09/2026, vigente a partir de 24/09/2026
bula, seção "Gravidez": "<trecho literal da seção>"
  fonte: bula publicada pela FDA (openFDA), set id ..., versão de 2026-03-14
```

Os valores acima são ilustrativos. Cada linha sai com a fonte, a data ou versão e o
endereço. O assistente redige a resposta a partir desses trechos; o servidor não
interpreta, não calcula e não recomenda.

## Licença

A DCB e a RENAME são publicadas sob CC BY-NC-SA 4.0. Os dados derivados delas sairão com
a mesma licença e com crédito à fonte, e não haverá plano pago sobre esses dados. A
licença do código será definida na primeira versão.

## Relação com outros projetos

É um projeto irmão do [medical-terminologies-mcp](https://github.com/SidneyBissoli/medical-terminologies-mcp),
que serve terminologias internacionais (ICD-11, CID-10, LOINC, RxNorm, ATC, MeSH) ao
pesquisador de qualquer país. Os dois públicos são atendidos por produtos separados.

## Autor

Sidney Bissoli, projeto pessoal, sem pessoa jurídica.
