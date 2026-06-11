# DataCentric-LegalNER-BR

Repositório de artefatos do projeto **DataCentric-LegalNER-BR**, voltado ao reconhecimento de entidades nomeadas (REN) no domínio jurídico brasileiro com abordagem centrada em dados.

## Conteúdo do repositório

Os arquivos estão agrupados abaixo pela função que cumprem no pipeline descrito no artigo.

### Dados e taxonomia

**`100_amostras_gold_standard_reformulado.jsonl`**
Cem segmentos textuais com anotações de entidades em JSONL. Serve para ilustrar a estrutura do acervo e o formato esperado das saídas.

> **Aviso:** este conjunto foi anonimizado e modificado para publicação. Não é o mesmo gold standard utilizado na avaliação oficial do estudo.

**`distribuicao_categorias_amostras_gold_standard_real.jsonl`**
Distribuição quantitativa e percentual das categorias de entidades no gold standard real. O conteúdo está em texto com formatação markdown.

**`classes_entidades.md`**
Tabela das 32 classes do projeto: descrição, exemplos de formatação, frequência no corpus e F1 por classe.

**`modelo_dados.json`**
Esquema JSON que define campos, tipos e proveniência dos registros produzidos na extração de entidades.

**`diretrizes_anotacao_ner.pdf`**
Manual de anotação com regras, critérios e exemplos para o escopo jurídico trabalhista.

### Pré-rotulagem (cold start)

**`prompt_qwen_2.5_70b.md`**
Prompt estruturado usado no `Qwen/Qwen2.5-70B-Instruct` para pré-anotação local de segmentos jurídicos.

**`selecao_llm_pre_rotulacao.md`**
Complemento à Seção 3.1 do artigo. Registra a exploração qualitativa que levou à escolha do Qwen-2.5-72B como pré-rotulador do acervo autêntico, incluindo critérios de seleção e modelos inspecionados.

**`analise_modelos.png`**
Figura com comparação lado a lado das saídas dos LLMs avaliados. O excerto exibido é fictício, sem texto de processo real.

### Geração sintética

**`geracao_sintetica.md`**
Complemento à Seção 3.1. Resume o fluxo de geração sintética com Gemini 2.5 Flash, pós-processamento, normalização para as 32 classes e auditoria humana. Aponta para os dois arquivos de prompt abaixo.

**`prompt_sintetico_entidades.md`**
Prompt da trilha de entidades administrativas e estruturadas. Gera textos fictícios a partir de foco temático, estilo e complexidade.

**`prompt_sintetico_lgpd.md`**
Prompt da trilha de categorias sensíveis (LGPD). Gera amostras sintéticas com entidades do art. 11, em modo unitário ou em lote.

### Modelo encoder

**`bertimbau_base_vs_large.md`**
Complemento à Seção 4.1 do artigo. Documenta a comparação entre BERTimbau Base e Large treinados com o mesmo protocolo, com tabelas por fold e a justificativa da escolha da variante Large.
