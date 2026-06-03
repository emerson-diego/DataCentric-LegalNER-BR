# Comparação BERTimbau Base vs Large

Este arquivo complementa a **Seção 4.1 (*Modelo Base*)** do artigo. Ele registra por que o estudo adotou o BERTimbau em português brasileiro, por que a variante **Large** foi escolhida em detrimento da **Base**, e quais foram os números obtidos quando as duas variantes foram treinadas com o **mesmo protocolo**.

## Contexto

O artigo descreve um pipeline de curadoria de corpus e a especialização de um modelo de reconhecimento de entidades (REN) na Justiça do Trabalho. Na Seção 4.1, comparamos duas variantes do BERTimbau — Base e Large — e adotamos a Large. **Neste arquivo estão** o protocolo, as tabelas por *fold* e a leitura dos resultados.

**Importante:** as métricas da **Seção 5** e do **Gold Standard** (por exemplo, F1 micro de 98,15% na avaliação cega) referem-se ao **modelo de produção** do manuscrito. Os F1 absolutos abaixo vêm do experimento comparativo Base/Large; o modelo de produção da Seção 5.1 reflete o corpus e os ciclos finais de curadoria e pode apresentar médias ligeiramente distintas (por exemplo, F1 micro médio de 97,76% na validação cruzada reportada no artigo). Os valores deste arquivo servem sobretudo para quantificar o **ganho relativo entre Base e Large** sob o mesmo protocolo.

## Modelos comparados

| Variante | Identificador Hugging Face |
|----------|----------------------------|
| Base | `neuralmind/bert-base-portuguese-cased` |
| Large | `neuralmind/bert-large-portuguese-cased` |

Ambos são encoders monolíngues em português brasileiro (Souza et al., 2020), ajustados por *fine-tuning* na mesma taxonomia de **32 classes** do estudo.

## Protocolo experimental

A comparação seguiu o mesmo arranjo descrito na Tabela de hiperparâmetros do artigo:

| Item | Valor |
|------|--------|
| Taxonomia | 32 classes (REN no domínio jurídico-trabalhista) |
| Partição | 10% teste reservado; 90% com validação cruzada estratificada (*k* = 10) |
| Semente | 42 |
| Comprimento máximo | 512 *tokens* |
| Métrica | F1 com *exact match* em nível de entidade |
| Seleção do melhor modelo | Maior `eval_f1_micro` em cada *fold* |
| Otimização | AdamW, LR 2×10⁻⁵, *weight decay* 0,03, *warmup* 500 passos, decaimento cosseno, lote efetivo 64 |
| *Early stopping* | Paciência 2; Δloss ≥ 0,001 |
| Hardware | 2× NVIDIA L40S (46 GB VRAM) |

## Resultados agregados

### Validação cruzada (10 *folds*)

| Métrica | Base | Large | Diferença (Large − Base) |
|---------|------|-------|--------------------------|
| F1 micro (média ± desvio) | 97,41% ± 0,11% | 97,89% ± 0,06% | +0,48 p.p. |
| F1 macro (média ± desvio) | 95,94% ± 0,21% | 96,58% ± 0,14% | +0,64 p.p. |
| F1 micro (mín. / máx.) | 97,20% / 97,55% | 97,79% / 98,01% | — |
| F1 macro (mín. / máx.) | 95,58% / 96,21% | 96,35% / 96,76% | — |

### Teste reservado (10% do acervo)

| Métrica | Base | Large | Diferença |
|---------|------|-------|-----------|
| F1 micro | 97,32% | 97,95% | +0,63 p.p. |
| F1 macro | 96,00% | 96,90% | +0,90 p.p. |

O ganho absoluto é **modesto** em relação à variação entre *folds*, mas o Large ficou **à frente do Base em todos os dez *folds* no F1 macro** (tabela abaixo).

## Resultados por *fold*

Valores em percentual (F1 × 100), duas casas decimais.

| *Fold* | Base F1 micro | Large F1 micro | Δ micro | Base F1 macro | Large F1 macro | Δ macro |
|-------|---------------|----------------|---------|---------------|----------------|---------|
| 1 | 97,30 | 97,89 | +0,59 | 95,87 | 96,67 | +0,80 |
| 2 | 97,31 | 97,79 | +0,48 | 95,63 | 96,54 | +0,90 |
| 3 | 97,40 | 97,94 | +0,54 | 95,77 | 96,42 | +0,64 |
| 4 | 97,40 | 97,90 | +0,50 | 96,08 | 96,66 | +0,58 |
| 5 | 97,54 | 98,01 | +0,47 | 95,99 | 96,76 | +0,77 |
| 6 | 97,20 | 97,83 | +0,63 | 95,58 | 96,35 | +0,77 |
| 7 | 97,47 | 97,92 | +0,45 | 96,11 | 96,70 | +0,60 |
| 8 | 97,52 | 97,81 | +0,29 | 96,21 | 96,36 | +0,16 |
| 9 | 97,36 | 97,85 | +0,48 | 95,98 | 96,69 | +0,71 |
| 10 | 97,55 | 97,93 | +0,38 | 96,18 | 96,66 | +0,47 |

## Como interpretar

- **Base vs Large:** em média, o Large ganhou cerca de **0,5 p.p.** em F1 micro e **0,6 p.p.** em F1 macro na validação cruzada — números citados de forma resumida na Seção 4.1 do artigo.
- **Consistência:** o Large não perdeu para o Base no F1 macro em nenhum *fold*.
- **Custo-benefício:** o incremento é pequeno, mas estável; o artigo ainda assim adota o Large pela maior capacidade do encoder em 32 classes com fronteiras contextuais.
- **Inferência:** na Seção 5.2 do artigo, o modelo Large especializado mantém inferência viável em *fp16* com menos de 4 GB de VRAM por segmento.

## Por que BERTimbau (e não um encoder multilíngue genérico)

O corpus e as 32 classes são **exclusivamente em português brasileiro**, com vocabulário da Justiça do Trabalho. O BERTimbau foi pré-treinado nesse idioma e é referência usual em classificação e REN em PT-BR. Encoders multilíngues generalistas (por exemplo, XLM-R) priorizam cobertura ampla de idiomas, não léxico jurídico nacional. Como o estudo é monolíngue, **não houve benchmark** com encoders multilíngues nem com modelos jurídicos de outros domínios nesta versão do trabalho.

## Referência

Souza, F.; Nogueira, R.; Lotufo, R. BERTimbau: Pretrained BERT Models for Brazilian Portuguese. BRACIS 2020.

## Origem dos números

Métricas consolidadas a partir de execuções registradas no MLflow (experimento comparativo, 2026). Fonte suplementar citada na Seção 4.1 do artigo.
