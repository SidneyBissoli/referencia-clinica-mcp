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
