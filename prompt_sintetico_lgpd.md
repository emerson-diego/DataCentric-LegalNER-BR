Instruções para o bloco `tipo=lgpd`: geração de amostras sintéticas (modos unitário e em lote) com até 512 tokens, contendo entidades sensíveis conforme os rótulos do esquema abaixo.

Posteriormente, por decisão taxonômica da equipe do projeto, o esquema intermediário de seis rótulos (`FILIACAO_ORGANIZACIONAL`, `DADO_GENETICO_BIOMETRICO`, etc.) foi consolidado nas quatro classes sensíveis do art. 11 presentes no acervo final: `DADO_GENETICO_BIOMETRICO` foi descontinuado e menções de `FILIACAO_ORGANIZACIONAL` passaram a ser tratadas, conforme o contexto, em `OPINIAO_POLITICA` ou `CONVICCAO_RELIGIOSA`.

## GERACAO_AMOSTRA_LGPD (prompt unitário)
Você é um especialista em geração de dados sintéticos com entidades sensíveis conforme a LGPD.

TAREFA: Gere UM texto sintético com até 512 tokens e anote TODAS as entidades sensíveis presentes no texto.

TEMA/CONTEXTO:
"{tema_contexto}"

# SCHEMA FECHADO — USE APENAS ESTES 6 RÓTULOS NO CAMPO "label"
- ORIGEM_RACIAL_ETNICA: raça, cor, ascendência ou origem nacional/étnica
  Exemplos: branca, parda, preta, negra, indígena, amarela, quilombola, cigana, homem negro, descendente de japoneses
- CONVICCAO_RELIGIOSA: crença, fé ou ausência dela
  Exemplos: católico praticante, umbandista, evangélico da Assembleia de Deus, espírita kardecista, judeu ortodoxo, muçulmano, ateu convicto
- OPINIAO_POLITICA: posicionamentos políticos (NÃO filiação formal a partido/sindicato)
  Exemplos: militante do movimento estudantil, participante de manifestação pela democracia, apoiador declarado de candidato, defensor do liberalismo, crítico do governo em redes sociais, intenção de voto declarada
- FILIACAO_ORGANIZACIONAL: associação FORMAL a sindicato, partido político ou organização religiosa/filosófica
  Exemplos: filiado ao PT, membro do PL, dirigente do Sindicato dos Metalúrgicos, associado ao Movimento Brasil Livre (MBL), membro da maçonaria, dizimista na Igreja Universal
- DADO_SAUDE_VIDA_SEXUAL: saúde física/mental, orientação sexual, identidade de gênero
  Exemplos: portador de HIV, diagnóstico de câncer, transtorno bipolar, depressão crônica, esquizofrenia, epilepsia, laudo de invalidez, atestado de doença grave, tratamento psiquiátrico
- DADO_GENETICO_BIOMETRICO: dados genéticos ou biométricos vinculados a pessoa identificável
  Exemplos: exame de DNA, mapeamento genético, sequenciamento de genoma, reconhecimento facial, impressão digital, padrão de voz, geometria da mão

INSTRUÇÕES:
1. O texto gerado deve ter no máximo 512 tokens
2. Use SOMENTE um dos 6 rótulos acima — nenhum outro label é permitido
3. O texto deve conter entidades sensíveis reais no contexto narrativo
4. Inclua variações linguísticas, sinônimos e expressões naturais de documentos
5. O campo "text" de cada entidade deve ser o trecho exato presente no texto gerado
6. Retorne APENAS o JSON com o texto e a lista de entidades

FORMATO DE RESPOSTA (JSON):
{
  "text": "texto_sintetico_com_ate_512_tokens",
  "entities": [
    {"text": "segmento_exato_do_texto", "label": "ORIGEM_RACIAL_ETNICA"},
    {"text": "outro_segmento", "label": "CONVICCAO_RELIGIOSA"}
  ]
}

---

## GERACAO_AMOSTRAS_LGPD_LOTE (prompt em lote)
Você é um especialista em geração de dados sintéticos com entidades sensíveis conforme a LGPD.

TAREFA: Gere {quantidade_documentos} amostras sintéticas, cada uma com até 512 tokens, e anote TODAS as entidades sensíveis em cada texto.

TEMAS/CONTEXTOS:
{temas_combinados}

# SCHEMA FECHADO — USE APENAS ESTES 6 RÓTULOS NO CAMPO "label"
- ORIGEM_RACIAL_ETNICA: raça, cor, ascendência ou origem nacional/étnica
- CONVICCAO_RELIGIOSA: crença, fé ou ausência dela
- OPINIAO_POLITICA: posicionamentos políticos (NÃO filiação formal)
- FILIACAO_ORGANIZACIONAL: associação FORMAL a sindicato, partido ou organização religiosa/filosófica
- DADO_SAUDE_VIDA_SEXUAL: saúde física/mental, orientação sexual, identidade de gênero
- DADO_GENETICO_BIOMETRICO: dados genéticos ou biométricos vinculados a pessoa identificável

INSTRUÇÕES:
1. Para CADA amostra, gere texto com no máximo 512 tokens
2. Use SOMENTE um dos 6 rótulos acima — nenhum outro label é permitido
3. Cada texto deve conter entidades sensíveis no contexto narrativo
4. Inclua variações linguísticas, sinônimos e expressões naturais de documentos
5. Retorne APENAS o JSON com texto e entidades de cada amostra

FORMATO DE RESPOSTA (JSON):
{
  "amostra_1": {
    "text": "texto_sintetico_com_ate_512_tokens",
    "entities": [
      {"text": "segmento_exato_do_texto", "label": "DADO_SAUDE_VIDA_SEXUAL"}
    ]
  },
  "amostra_2": {
    "text": "texto_sintetico_com_ate_512_tokens",
    "entities": [
      {"text": "entidade_encontrada", "label": "FILIACAO_ORGANIZACIONAL"}
    ]
  }
}
