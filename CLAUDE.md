# LAB3 — Análisis de Atención en Transformers (BERT)

## Objetivo del laboratorio

Inspeccionar matrices de atención de `bert-base-multilingual-cased` sobre 2 páginas
consecutivas de *La Metamorfosis* (Kafka), estudiando cómo distintas capas y cabezas
distribuyen atención entre tokens. No se busca afirmar causalidad; se busca lectura
crítica de las representaciones internas.

## Estructura del repo

```
.
├── data/
│   └── corpus/
│       ├── la-metamorfosis.pdf       # PDF fuente completo
│       ├── pagina_1.txt               # página consecutiva 1 (extraída)
│       └── pagina_2.txt               # página consecutiva 2 (extraída)
├── notebooks/
│   └── attention_analysis.ipynb       # notebook único, ejecutado, entregable final
├── results/
│   └── ...                            # tablas/figuras exportadas del notebook
├── requirements.txt
├── .gitignore
└── CLAUDE.md
```

## Setup

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Modelo: `bert-base-multilingual-cased` (soporta español, encoder-only).

## División del trabajo (2 personas)

El notebook es único y compartido; cada persona es dueña de sus secciones pero ambas
revisan el resultado final antes de entregar.

### Persona 1 — Datos, tokenización, ejecución del modelo (Partes A y B)

- Extraer 2 páginas consecutivas al azar de `data/corpus/la-metamorfosis.pdf` y
  guardarlas como texto plano en `data/corpus/pagina_1.txt` y `pagina_2.txt`.
- Seleccionar 2+ oraciones de interés dentro de esas páginas (idealmente con algún
  contraste léxico/semántico interesante, ver Parte D).
- Parte A: cargar tokenizer, mostrar tokens por oración, identificar `[CLS]`/`[SEP]`,
  subpalabras (`##`), y diferencias entre palabras lingüísticas y tokens del modelo.
- Parte B: cargar el modelo con `output_attentions=True`, correr forward pass,
  reportar shape de `outputs.attentions` (por capa: `(batch, n_heads, n_tokens, n_tokens)`).
- Dejar utilidades reutilizables (tokenizar, correr modelo, obtener attentions) en
  celdas claras al inicio del notebook para que Persona 2 las use en Partes C/D.

### Persona 2 — Análisis de atención, comparación, preguntas (Partes C, D y 6)

- Parte C: para 2 capas × 2 cabezas × 2 tokens lingüísticamente relevantes por oración,
  extraer y tabular (pandas) los 5 tokens con mayor peso de atención.
- Parte D: comparar patrones de atención entre las 2 (o más) oraciones seleccionadas,
  explicando si/cómo cambia la atención según contexto (ej. polisemia tipo
  "banco" financiero vs. "banco" de parque, si aparece un caso análogo en el corpus).
- Responder por escrito las 7 preguntas de análisis de la sección 6 del enunciado,
  apoyándose en las tablas/figuras generadas.
- Exportar tablas y figuras clave a `results/`.

### Entrega conjunta

- Notebook `.ipynb` corrido de principio a fin sin errores (Kernel → Restart & Run All).
- Ambas partes citan el mismo corpus y mismas oraciones para que el análisis sea coherente.
