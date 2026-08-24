# DataCentric-LegalNER-BR

Repositório suplementar do artigo *Um Pipeline Data-Centric para Construção de Corpus de Reconhecimento de Entidades Nomeadas para o Domínio Trabalhista Brasileiro* (SBBD 2026). Reúne artefatos metodológicos e de dados do estudo de REN na Justiça do Trabalho com abordagem centrada em dados.

O manuscrito descreve um pipeline que integra geração sintética, supervisão fraca, aprendizado ativo e validação humana, resultando em um corpus de treinamento com 53.324 segmentos anotados em 32 classes e um Gold Standard independente de 100 segmentos reais (2.605 entidades). Por sigilo judicial, o acervo integral e os textos originais do Gold Standard não são publicados. Este repositório disponibiliza diretrizes, esquemas, prompts, amostras anonimizadas e documentação complementar para reprodutibilidade parcial.

## Reprodutibilidade das métricas

As métricas reportadas no artigo (F1 micro de 98,15% no Gold Standard, F1 por classe, latência etc.) referem-se ao modelo e ao conjunto de avaliação usados na redação do manuscrito. **Não garantimos a reprodução exata desses valores** em inferências posteriores, por dois motivos:

1. **Amostra publicada ≠ Gold Standard original.** O arquivo `100_amostras_gold_standard_reformulado.jsonl` foi anonimizado e alterado para divulgação. Embora preserve a estrutura e a taxonomia, contém variações textuais e de anotação em relação aos 100 segmentos reais do PJe e PROAD usados na avaliação cega (Seção 4.3). A distribuição de categorias no GS real está em `distribuicao_categorias_amostras_gold_standard_real.jsonl`, mas os textos originais não são publicados.

2. **Modelo em evolução contínua.** O pipeline descrito no artigo opera com retreinamento e curadoria iterativos. O modelo especializado em produção muda ao longo do tempo; pesos, corpus de treino e checkpoints não estão disponíveis neste repositório.

Para referência de métricas por classe no Gold Standard (suporte e F1 de uma avaliação pontual), consulte `classes_entidades_gold_standard.md`. Os valores lá registrados são indicativos e podem divergir dos reportados no artigo conforme a versão do modelo e o protocolo de avaliação.

## Artigo

**`artigo_sbbd_2026.pdf`**
Versão em PDF do manuscrito submetido ao SBBD 2026. É a referência principal deste repositório: as seções citadas abaixo seguem a numeração desse arquivo.

## Conteúdo do repositório

Cada arquivo abaixo corresponde a uma menção explícita no artigo. Entre parênteses, a seção do manuscrito.

### Dados e taxonomia

**`100_amostras_gold_standard_reformulado.jsonl`** (Seções 3.4 e 4.3)
Cem segmentos em JSONL com anotações de entidades. Ilustra a estrutura do acervo e o formato de saída esperado.

> **Aviso:** amostra reformulada, anonimizada e alterada para publicação. Não reproduz os 100 segmentos reais do PJe e PROAD usados na avaliação cega (Seção 4.3), nem contém textos sigilosos. Por conter variações em relação ao dataset original, **não deve ser usada para replicar as métricas do artigo** (ver seção [Reprodutibilidade das métricas](#reprodutibilidade-das-métricas)).

**`distribuicao_categorias_amostras_gold_standard_real.jsonl`** (Seções 3.4 e 4.3)
Distribuição quantitativa e percentual das 32 categorias no Gold Standard real (100 segmentos, 2.605 entidades). O conteúdo está em texto com formatação markdown.

**`classes_entidades_gold_standard.md`** (Seções 3.4 e 5.1)
Detalhamento das 32 classes no Gold Standard: suporte anotado e F1 por classe de uma avaliação pontual. Complementa `classes_entidades.md`, cujos F1 referem-se ao corpus de treino. Sujeito às limitações descritas em [Reprodutibilidade das métricas](#reprodutibilidade-das-métricas).

**`classes_entidades.md`** (Seção 3.4)
Detalhamento das 32 classes: definição semântica, exemplos, frequência absoluta no corpus de treino e F1 individual por classe.

**`distribuicao_classes.png`** (Seção 3.4)
Figura com a distribuição de suporte e percentual das 32 classes no corpus de treinamento. Duas tabelas lado a lado, com mapa de calor na coluna de suporte. Os valores correspondem a `classes_entidades.md` (547.036 entidades no total). As classes sensíveis da LGPD aparecem ao final da tabela da direita.

**`modelo_dados.json`** (Seção 3.3)
Especificação do esquema JSONL adotado na curadoria: texto, entidades anotadas, metadados de proveniência e histórico de auditoria humana.

**`diretrizes_anotacao_ner.pdf`** (Seção 3.2)
Manual técnico de anotação: taxonomia, critérios de fronteira, resolução de ambiguidades e alinhamento entre revisores.

### Pré-rotulagem e baseline generativo

**`prompt_qwen_2.5_70b.md`** (Seções 3.1 e 5.1)
Prompt estruturado usado no Qwen-2.5-72B para pré-rotulagem local do acervo autêntico e, na avaliação do artigo, como baseline generativo no Gold Standard (protocolo unificado da Seção 4.4).

**`selecao_llm_pre_rotulacao.md`** (Seção 3.1)
Exploração qualitativa entre LLMs abertos executados localmente que antecedeu a escolha do Qwen-2.5-72B como pré-rotulador no cold start.

**`analise_modelos.png`** (Seção 3.1)
Comparação visual lado a lado das saídas dos LLMs inspecionados. O excerto é fictício, sem texto de processo real.

### Geração sintética

**`geracao_sintetica.md`** (Seção 3.1)
Fluxo de geração sintética com Gemini 2.5 Flash (única etapa com API externa do projeto), pós-processamento, normalização para as 32 classes e auditoria humana. Volume no corpus final: 15.877 segmentos sintéticos (~29,8% do treino).

**`prompt_sintetico_entidades.md`** (Seção 3.1)
Prompt da trilha de entidades administrativas e estruturadas. Entrada com taxonomia e cenários inventados, sem peças do PJe ou PROAD.

**`prompt_sintetico_lgpd.md`** (Seção 3.1)
Prompt da trilha de categorias sensíveis (LGPD, art. 11). Geração em modo unitário ou em lote, com auditoria humana intensiva nos sintéticos LGPD (68,4% revisados, conforme Seção 3.4).

### Modelo encoder

**`bertimbau_base_vs_large.md`** (Seção 4.1)
Comparação entre BERTimbau Base e Large no mesmo protocolo de validação cruzada (dez folds). Inclui métricas agregadas e valores por fold que fundamentam a escolha da variante Large. Os F1 absolutos deste experimento diferem dos do modelo de produção reportados na Seção 5.

## O que não está neste repositório

Conforme as limitações do artigo (Seção 6):

O corpus de treinamento completo (53.324 segmentos, ~107 MB), incluindo segmentos de documentos sigilosos.

Os textos originais do Gold Standard de avaliação cega.

Código do pipeline (Airflow, rotulador, treinamento) e pesos do modelo especializado (previstos como trabalho futuro no manuscrito).
