# El Guardián — ejemplos comentados

Casos ilustrativos para entender la tarea. Vuestro bot recibe
`{"pregunta", "fuentes", "respuesta"}` (sin etiquetas) y devuelve un score en
`[0,1]` = probabilidad de que la respuesta **afirme algo que las fuentes no
respaldan**. Aquí mostramos la etiqueta y el porqué.

> Regla de oro: una respuesta es **fiel (0)** solo si *todo* lo que afirma está
> respaldado por las fuentes. Basta **una** afirmación no respaldada para que
> sea **alucinación (1)**.

## Fiel (0) — respaldada palabra por palabra
- **Fuente:** «El maratón recorre 42 kilómetros por el centro de la ciudad.»
- **Respuesta:** «El maratón recorre 42 kilómetros por el centro de la ciudad.»
- **Por qué 0:** coincide con la fuente.

## Fiel (0) — paráfrasis (¡trampa para el conteo de palabras!)
- **Fuente:** «La Constitución española fue aprobada en 1978.»
- **Respuesta:** «El texto constitucional español se aprobó en mil novecientos
  setenta y ocho.»
- **Por qué 0:** dice lo mismo con otras palabras y el número en letra. Un
  detector que mida solape léxico la marcará como sospechosa **por error**.

## Contradicción (1)
- **Fuente:** «El maratón recorre 42 kilómetros por el centro de la ciudad.»
- **Respuesta:** «El maratón recorre 42 kilómetros por el extrarradio.»
- **Por qué 1:** la fuente dice *centro*, la respuesta dice *extrarradio*.

## Distorsión (1) — entidad correcta, dato equivocado
- **Fuente:** «El maratón recorre 42 kilómetros por el centro de la ciudad.»
- **Respuesta:** «El maratón recorre 97 kilómetros por el centro de la ciudad.»
- **Por qué 1:** el número no es el de la fuente. Ojo: solapa *casi del todo*,
  así que el conteo de palabras la da por buena **por error**.

## No respaldado (1) — añade algo que la fuente no dice
- **Fuente:** «El maratón recorre 42 kilómetros por el centro de la ciudad.»
- **Respuesta:** «El multitudinario maratón recorre 42 kilómetros por el centro
  de la ciudad.»
- **Por qué 1:** «multitudinario» no está en la fuente. Es un inciso **sutil**
  embebido en una frase por lo demás correcta: cuesta pillarlo.

## Sobre-generalización (1)
- **Fuente:** «El parque protege varias especies de aves.»
- **Respuesta:** «El parque protege todas las especies de aves.»
- **Por qué 1:** la fuente dice *varias*, la respuesta dice *todas*.

## Aguja en el pajar (1) — frase falsa enterrada en respuesta larga
- **Fuentes:** «El ensayo incluyó a 1.800 participantes.» · «El fármaco redujo la
  presión un 12 %.» · «Los efectos adversos fueron leves.»
- **Respuesta:** «El ensayo incluyó a 1.800 participantes. El fármaco redujo la
  presión un **21 %**. Los efectos adversos fueron leves.»
- **Por qué 1:** dos de tres frases son fieles, pero una (el 21 %) es falsa. Si
  **promediáis** la señal sobre toda la respuesta, la frase falsa se diluye y se
  os cuela. Hay que **descomponer por afirmaciones** y verificar cada una.

## Distorsión por atribución cruzada (1) — el dato existe, pero es de otro
- **Fuentes:** «La empresa A facturó 42 millones.» · «La empresa B facturó 18 millones.»
- **Respuesta:** «La empresa B facturó 42 millones.»
- **Por qué 1:** el 42 aparece en las fuentes, pero es de A. Buscar si el número
  «está en las fuentes» no basta: hay que comprobar a quién se refiere.

## Contradicción sin «no» (1)
- **Fuente:** «La demanda de vivienda aumentó un 8 % respecto al año anterior.»
- **Respuesta:** «La demanda de vivienda se redujo un 8 % respecto al año anterior.»
- **Por qué 1:** «aumentó» frente a «se redujo». No hay ninguna negación que
  buscar: hay que entender la relación.

## Caso con varias fuentes — respuesta que las combina
Así son muchos casos del test: varias fuentes (con datos que sobran) y una
respuesta que **combina** varias de ellas en pocas frases.

- **Fuentes:**
  1. «La biblioteca municipal abrió en 1987.»
  2. «Tiene un fondo de 64.000 volúmenes.»
  3. «El año pasado registró 21.000 préstamos.»
  4. «Organiza un club de lectura los jueves.»
  5. «La sala de estudio cierra los domingos.»
- **Respuesta FIEL (0):** «Con 64.000 volúmenes en su fondo, la biblioteca prestó
  21.000 libros el año pasado.»
  → Todo está respaldado, pero **ninguna fuente lo dice entero**: junta las
  fuentes 2 y 3. Si comparáis la frase con cada fuente por separado, ninguna la
  respalda del todo y daréis falsa alarma. Verificad cada afirmación contra
  **todas** las fuentes juntas.
- **Respuesta INFIEL (1):** «La biblioteca, que abrió en 1987 gracias a una
  donación privada, tiene 64.000 volúmenes.» → nada dice por qué abrió
  (no respaldado).

---

## Criterio de etiquetado

Es el criterio exacto con el que se han etiquetado train, test y giro.

**Fiel** = *cada* afirmación está respaldada por las fuentes, tomadas en
conjunto. Basta **una** afirmación sin respaldo para que sea alucinación. Cuenta
lo que dicen las fuentes, no lo que es verdad en el mundo.

Una respuesta fiel **puede**: parafrasear con sinónimos exactos; reordenar y
combinar datos de varias fuentes; omitir información; hacer aritmética directa
con cifras de las fuentes (sumar, «el doble», 40 % = «cuatro de cada diez»);
cambiar de unidad sin perder precisión (2.000 m = 2 km).

Una respuesta fiel **no puede**: endurecer un verbo («vacunó» ≠ «inmunizó»,
«propuso» ≠ «aprobó», «el presupuesto es» ≠ «costará»); quitar un matiz («más de
150.000» ≠ «150.000»); ampliar el alcance («algunos» ≠ «todos»); añadir causas,
motivos o consecuencias; cambiar quién hizo qué.

---

### Qué os enseña esto
1. **Las palabras en común engañan por los dos lados**: las paráfrasis fieles
   comparten pocas palabras con la fuente y las distorsiones comparten casi todas.
2. **Descomponed en afirmaciones** y verificad cada una contra el conjunto de
   fuentes; no promediéis.
3. **Comprobad a quién se refiere cada dato**, no solo si aparece.
4. **Calibrad el corte**: dejar pasar una alucinación pesa **4 veces** más que
   una falsa alarma.
