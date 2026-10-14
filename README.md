# El Guardián — material del reto

Bienvenidos al hackathon. Vais a construir un **detector de alucinaciones**: ante la
respuesta de una IA y sus fuentes, vuestro bot decide si la respuesta afirma algo
que las fuentes **no respaldan**.

Solo necesitáis **Python 3.9 o superior**. No se instala ni se ejecuta ningún modelo.

## 1. Leed el enunciado

**[Enunciado_El_Guardian.docx](Enunciado_El_Guardian.docx)**: el problema, un caso de
ejemplo, qué recibís, cómo se puntúa y el calendario. Pulsad *Download* (o el icono
de descarga) para abrirlo en Word.

## 2. Descargad el kit

- Sin git: botón verde **Code → Download ZIP**, o descargad directamente
  [`El_Guardian_kit_alumnos.zip`](El_Guardian_kit_alumnos.zip).
- Con git: `git clone https://github.com/jotajota1302/material.git`

Todo el código y los datos están en la carpeta [`kit/`](kit/) (el zip contiene lo mismo). Leed [`kit/README.md`](kit/README.md) (reglas,
señales, métrica) y [`kit/EJEMPLOS.md`](kit/EJEMPLOS.md) (qué cuenta como alucinación).

| Fichero | Para qué |
|---|---|
| `kit/README.md` | Reglas, señales, entrega y puntuación. |
| `kit/EJEMPLOS.md` | Casos comentados de cada tipo de alucinación. |
| `kit/ejemplo_bot.py` | Un bot que ya funciona: vuestro punto de partida. |
| `kit/evaluar_local.py` | Mide vuestro bot sobre `train.jsonl` con la métrica del leaderboard. |
| `kit/generar_predicciones.py` | Genera el CSV que entregáis. |
| `kit/train.jsonl` | Casos etiquetados para desarrollar y calibrar. |
| `kit/test_publico.jsonl` | Los casos que hay que puntuar (sin etiquetas). |

## 3. Primeros pasos

```
cd kit
python evaluar_local.py --bot ejemplo_bot.py --datos train.jsonl
```

Copiad `ejemplo_bot.py` a `mi_bot.py`, mejoradlo y medid cada cambio con `evaluar_local.py`.

## 4. Calendario

| Hora | Qué pasa |
|---|---|
| Inicio | Material disponible. Hasta 3 entregas (solo test). |
| **14:00** | **Giro:** aparecerá aquí `kit/giro_publico.jsonl`. Desde entonces cada entrega lleva test + giro en un solo CSV. Hasta 3 entregas más. |
| **18:00** | Cierre de entregas. Se revela la clasificación final. |
| Día 2 | El top-10 reproduce su bot sobre un lote nuevo y lo defiende ante el jurado. |

A las 14:00 actualizad el repositorio (`git pull`, o volved a descargar el ZIP).

## 5. Cómo entregar

Cada entrega es **`predicciones.csv` + `mi_bot.py`**, en la carpeta
[`entregas/`](entregas/) de este repositorio, mediante un *Pull Request*.
Las instrucciones paso a paso están en [`entregas/README.md`](entregas/README.md).
