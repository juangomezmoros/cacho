# Reglas del juego: Cacho

Este documento describe las reglas exactas del juego de dados **Cacho** (variante similar a Liar's Dice / Dudo / Perudo, pero con reglas específicas de nuestro grupo). Estas son las reglas oficiales para esta implementación.

---

## 1. Configuración inicial

- **Jugadores:** entre 2 y 6.
- **Dados por jugador:** cada jugador comienza con **5 dados** de seis caras.
- **Cubilete:** cada jugador tira sus dados de forma oculta, sin que los demás puedan verlos.
- **Inicio del juego:** antes de la primera ronda, todos los jugadores hacen una tirada inicial con un dado. **Quien saque el dado más alto comienza la primera ronda.** En caso de empate, los empatados vuelven a tirar entre ellos.

---

## 2. Estructura de una ronda

1. Todos los jugadores con dados los tiran de forma oculta.
2. El jugador que inicia la ronda **elige la dirección de juego** (horario o antihorario). Esa dirección se mantiene durante toda la ronda.
3. El jugador inicial hace la primera apuesta (cualquier apuesta inicial es válida).
4. Los siguientes jugadores, en la dirección elegida, deben elegir una de las siguientes acciones en su turno:
   - **Subir la apuesta**
   - **Dudar** la apuesta anterior
   - **Cazar** (apostar que la cantidad es exacta)
   - **Declarar salpicón** (solo si tiene 5 dados y no lo ha hecho en esta ronda)
5. La ronda termina cuando alguien duda o caza. Entonces se revelan los dados y se aplican las consecuencias.

---

## 3. Las apuestas

### 3.1 Formato

Una apuesta consiste en **cantidad + valor**. Ejemplo: `"tres cuatros"` significa "afirmo que entre todos los dados en mesa hay al menos 3 dados con valor 4".

### 3.2 El as (1) como comodín

**El 1 siempre es comodín**: cuenta como cualquier valor apostado. Por ejemplo, si la apuesta es "tres cuatros", todos los dados con valor 4 más todos los unos en la mesa cuentan para verificar si la apuesta es verdadera.

**Excepción:** cuando la apuesta es directamente sobre ases (1s), los unos cuentan solo como unos (no aplica el comodín en sí mismo).

### 3.3 Cómo subir una apuesta (valores normales)

Si la apuesta actual es `(N, V)` donde N es la cantidad y V es el valor (de 2 a 6), el siguiente jugador puede:

- **Subir cantidad** manteniendo el mismo valor: `(N+1, V)` o más.
- **Subir cantidad y cambiar valor**: `(N+k, V')` donde k ≥ 1 y V' puede ser cualquier valor de 2 a 6.
- **Mantener cantidad y subir el valor**: `(N, V')` donde V' > V.

**Ejemplos** (apuesta actual: "tres cuatros"):
- ✅ "cuatro cuatros" (sube cantidad)
- ✅ "cinco cuatros" (sube cantidad)
- ✅ "tres cincos" (mantiene cantidad, sube valor)
- ✅ "tres seis" (mantiene cantidad, sube valor)
- ✅ "cuatro doses" (sube cantidad, baja valor — válido porque subió la cantidad)
- ❌ "tres cuatros" (igual a la actual)
- ❌ "dos cincos" (baja cantidad)
- ❌ "tres treses" (mantiene cantidad y baja valor)

### 3.4 Cómo apostar ASES (1s)

Para pasar de una apuesta normal a una apuesta de ases, la cantidad de ases requerida es:

> **Ases requeridos = floor(cantidad_actual / 2) + 1**

Es decir, se divide la cantidad actual entre 2, se redondea hacia abajo, y se le suma 1.

**Tabla de referencia:**

| Cantidad apostada | Mínimo de ases para pasar |
|-------------------|---------------------------|
| 1 de algo         | 1 as                      |
| 2 de algo         | 2 ases                    |
| 3 de algo         | 2 ases                    |
| 4 de algo         | 3 ases                    |
| 5 de algo         | 3 ases                    |
| 6 de algo         | 4 ases                    |
| 7 de algo         | 4 ases                    |
| 8 de algo         | 5 ases                    |
| ...               | ...                       |

### 3.5 Subir DENTRO de los ases

Si la apuesta actual es de ases, se puede subir simplemente aumentando la cantidad: "3 ases" → "4 ases" → "5 ases", etc.

### 3.6 Salir de los ases hacia un valor normal

Si la apuesta actual es `K ases`, el siguiente jugador que quiera volver a apostar un valor normal debe apostar al menos:

> **Cantidad mínima = (K × 2) + 1**

Con cualquier valor de 2 a 6.

**Ejemplo:** si la apuesta es "3 ases", el siguiente jugador puede apostar "7 doses", "7 treses", ..., "7 seis", "8 doses", etc.

---

## 4. Dudar

Cualquier jugador, en su turno, puede **dudar** la apuesta del jugador anterior. Cuando alguien duda:

1. Todos los jugadores revelan sus dados.
2. Se cuenta cuántos dados cumplen la apuesta (incluyendo los unos como comodines, salvo que la apuesta haya sido sobre ases).
3. Se compara con la cantidad apostada:

| Resultado | Consecuencia |
|-----------|--------------|
| La apuesta era **correcta** (había al menos esa cantidad) | El que **dudó pierde 1 dado** |
| La apuesta era **incorrecta** (no había suficiente) | El que **apostó pierde 1 dado** |

**Quien pierde el dado inicia la siguiente ronda** y elige la dirección de juego.

---

## 5. Cazar (apostar exacto)

Cualquier jugador, en cualquier momento de su turno, puede **cazar**. Cazar significa afirmar que la cantidad de la apuesta actual es **exactamente** correcta (ni más ni menos).

1. Todos los jugadores revelan sus dados.
2. Se cuenta cuántos dados cumplen la apuesta (con comodines aplicables).
3. Se compara con la cantidad apostada:

| Resultado | Consecuencia |
|-----------|--------------|
| La cantidad era **exacta** | El que **cazó gana 1 dado** |
| La cantidad **no era exacta** (había más o menos) | El que **cazó pierde 1 dado** |

### 5.1 Regla del "dado de reserva" al cazar con 5 dados

Ningún jugador puede tener más de **5 dados activos** simultáneamente. Si un jugador con 5 dados acierta una caza, no recibe un sexto dado en ese momento, sino que **guarda un "dado de reserva"**. Cuando ese jugador pierda un dado en una ronda futura (por dudar mal, o por errar una apuesta o una caza), usa el dado de reserva en lugar de quedarse con 4. Es decir, el dado de reserva funciona como un **escudo contra la próxima pérdida**.

- El dado de reserva **no tiene fecha de vencimiento**: se conserva hasta que se use.
- Un jugador puede acumular varios dados de reserva si caza correctamente varias veces con 5 dados antes de perder alguno.

### 5.2 Quién inicia la siguiente ronda tras una caza

**Siempre inicia el cazador**, haya acertado o fallado la caza.

---

## 6. Salpicón

El **salpicón** es una jugada especial declarativa.

### 6.1 Condiciones para declarar salpicón

- El jugador debe tener exactamente **5 dados activos** (no aplica si tiene menos).
- El jugador no debe haber declarado salpicón ya en esta ronda (puede usarlo una vez por ronda como máximo, siempre que tenga 5 dados).
- Sus 5 dados deben tener **todos valores diferentes** (ej: 1-2-3-4-5, o 2-3-4-5-6).

### 6.2 Efectos del salpicón

- El salpicón es **solo declarativo**: el jugador no hace una apuesta nueva. Simplemente declara "salpicón" en su turno y pasa el turno al siguiente.
- La apuesta vigente sigue siendo **la que existía antes del salpicón**.
- El siguiente jugador **NO puede cazar**. Sus opciones son:
  - **Dudar el salpicón** (afirmar que el jugador no tenía realmente cinco valores diferentes).
  - **Subir la apuesta que venía antes del salpicón**, siguiendo las reglas normales.

### 6.3 Resultado de dudar el salpicón

| Resultado | Consecuencia |
|-----------|--------------|
| El salpicón era **falso** (no tenía 5 valores distintos) | El que **declaró salpicón pierde 1 dado** |
| El salpicón era **verdadero** | El que **dudó pierde 1 dado** |

> **Nota:** quien declaró un salpicón verdadero NO gana ningún dado.

---

## 7. Ronda especial: jugador con 1 dado

Cuando un jugador pierde su segundo dado y queda con **un solo dado**, hay una regla especial **únicamente en la ronda que ese jugador inicia**:

- **Los jugadores NO pueden ver sus propios dados** durante esa ronda.
- **Solo los jugadores que también tengan 1 dado pueden ver sus propios dados**.
- El resto de las reglas (apostar, dudar, cazar, salpicón si aplica) se mantienen igual.

En las rondas siguientes (que no inicie ese jugador) se juega normal. Si el jugador recupera dados (por cazar correctamente), la regla deja de aplicar.

---

## 8. Eliminación y victoria

- Un jugador es **eliminado** cuando pierde su último dado.
- El **ganador del juego** es el último jugador que quede con al menos un dado.

---

## 9. Resumen de las acciones por turno

En cada turno, el jugador puede hacer **una y solo una** de estas acciones:

| Acción | Condición | Efecto |
|--------|-----------|--------|
| **Apostar** | Si es el primer turno de la ronda, o para subir la apuesta vigente | Pasa el turno al siguiente |
| **Dudar** | Solo si hay una apuesta previa en la ronda (o un salpicón recién declarado) | Termina la ronda, se revelan dados |
| **Cazar** | Solo si hay una apuesta previa en la ronda Y el jugador anterior NO declaró salpicón | Termina la ronda, se revelan dados |
| **Declarar salpicón** | Solo si tiene 5 dados activos, no lo ha declarado esta ronda, y sus 5 dados tienen valores distintos | Pasa el turno al siguiente, que no puede cazar |

---

## 10. Casos borde y aclaraciones

- **Cuando se revelan los dados** (tras dudar o cazar), se cuentan los dados con valor igual al apostado **más** los unos (comodines), excepto si la apuesta era sobre ases.
- **Después de un salpicón**, el siguiente jugador no puede cazar. Si elige subir la apuesta vigente, el ciclo continúa normal y los siguientes jugadores ya pueden cazar de nuevo.
- **Empate en tirada inicial**: solo los empatados vuelven a tirar entre sí hasta que haya un único ganador.
