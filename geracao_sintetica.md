# Geração sintética (*cold start*)

Complemento à Seção 3.1 do artigo. Resume o que **não** está nos arquivos de *prompt*.

## Escopo

- **Modelo:** Gemini 2.5 Flash (única chamada a API externa do projeto).
- **Entrada:** taxonomia, parâmetros de lote e cenários **inventados** — nunca peças do PJe/PROAD.
- **Saída:** JSON com texto e entidades; depois normalização para as 32 classes e registro conforme `modelo_dados.json` (`proveniencia.tipo`: `lgpd` ou complementar).
- **Volume no corpus final:** 15.877 segmentos sintéticos (~29,8% do treino); 68,4% desse bloco com auditoria humana direta (§3.4 do artigo).

## Duas trilhas de *prompt*

| Trilha | Arquivo |
|--------|---------|
| Entidades administrativas e estruturadas | [`prompt_sintetico_entidades.md`](prompt_sintetico_entidades.md) |
| Categorias sensíveis (LGPD) | [`prompt_sintetico_lgpd.md`](prompt_sintetico_lgpd.md) |

Instruções de geração, diversidade e rótulos intermediários do gerador estão nos *prompts*. Na curadoria, o mapeamento para a taxonomia final segue `diretrizes_anotacao_ner.pdf`.

## Pós-processamento e auditoria

1. Validar JSON (sem markdown; rótulos dentro do *schema* do *prompt*).
2. Normalizar rótulos → 32 classes; calcular `start`/`end`.
3. Segmentar como os dados reais (NLTK; até 512 *tokens*).
4. Deduplicar e descartar exemplos inválidos.
5. Revisão humana, com ênfase em sintéticos LGPD (estereótipos e padrões identificáveis).

Textos reais do acervo seguem pré-rotulagem local (Qwen) e, depois, o *encoder* especializado — fora desta etapa sintética.
