# 🎲 Simulador Yahtzee — Método de Montecarlo

**Asignatura:** Simulación  
**Actividad:** Actividad Didáctica 2 — M1  
**Estudiante:** Santiago Londoño Rúa  
**Docente:** Julián Andrés Loaiza López  
**Institución:** Institución Universitaria Digital de Antioquia  
**Programa:** Ingeniería de Software y Datos  

---

## 📋 Descripción del proyecto

Este proyecto implementa un **simulador completo del juego Yahtzee clásico para 2 jugadores**, donde todas las decisiones de juego son tomadas automáticamente por un motor de inteligencia basado en el **método de Montecarlo** y modelado mediante **Cadenas de Markov**.

El objetivo académico es demostrar cómo el método de Montecarlo permite resolver problemas de toma de decisiones bajo incertidumbre mediante la simulación masiva de escenarios aleatorios, aplicando una **distribución de probabilidad uniforme U(1,6)** para el lanzamiento de los dados.

---

## 🎯 ¿Qué es el Yahtzee?

El Yahtzee es un juego de dados en el que cada jugador dispone de:
- **13 turnos** por partida
- **5 dados** de 6 caras por turno
- Hasta **3 lanzamientos** por turno
- **13 categorías** de puntuación, cada una usable una sola vez

Las dos decisiones clave de cada turno son:
1. ¿Qué dados guardar entre lanzamientos?
2. ¿En qué categoría anotar el resultado final?

Ambas decisiones se toman bajo incertidumbre, lo que hace del Yahtzee un caso de estudio ideal para el método de Montecarlo.

---

## 🧠 Métodos aplicados

### Método de Montecarlo
Ante cada decisión, el motor simula **miles de escenarios futuros aleatorios** para cada opción posible y elige la que produce el mayor **valor esperado** de puntuación.

- **Decisión de retención:** evalúa las 2⁵ = 32 combinaciones posibles de dados a guardar, simulando 6.000 relanzamientos por opción
- **Decisión de categoría:** evalúa las categorías disponibles estimando el costo de oportunidad mediante 800 simulaciones por categoría

### Cadena de Markov
El juego se modela como una **Cadena de Markov discreta**:
- **Estado:** combinación actual de dados guardados (tupla ordenada)
- **Transición:** resultado del relanzamiento de dados no guardados
- **Propiedad Markoviana:** el estado futuro depende solo del estado actual, no del historial

---

## 🏗️ Arquitectura del sistema

```
yahtzee_montecarlo.py
│
├── clase Dado              → distribución uniforme U(1,6)
├── clase Marcador          → 13 categorías + cálculo de puntajes
├── clase CadenaMarkov      → modelado de transiciones de estado
├── clase MotorMontecarlo   → decisiones por simulación masiva
├── clase Jugador           → nombre, marcador, historial de turnos
└── clase JuegoYahtzee      → orquestador de la partida completa
```

---

## 📁 Estructura del repositorio

```
yahtzee-montecarlo/
│
├── yahtzee_montecarlo.py          # Código fuente principal en Python
├── Yahtzee_Montecarlo.ipynb       # Notebook Google Colab (ejecución completa)
├── Londoño_Santiago_Montecarlo.docx  # Informe académico (introducción, metodología, resultados, conclusiones)
└── README.md                      # Este archivo
```

---

## ▶️ Cómo ejecutar el proyecto

### Opción 1 — Google Colab (recomendada)
1. Abrir el archivo `Yahtzee_Montecarlo.ipynb` en Google Colab
2. Ejecutar las celdas en orden (Menú → Entorno de ejecución → Ejecutar todo)
3. Los resultados, gráficos y análisis se generan automáticamente

### Opción 2 — Python local
```bash
# Clonar el repositorio
git clone https://github.com/TU_USUARIO/yahtzee-montecarlo.git
cd yahtzee-montecarlo

# Ejecutar el simulador
python yahtzee_montecarlo.py
```

**Requisitos:** Python 3.8 o superior. No requiere librerías externas para el código principal (`random`, `itertools` y `collections` son parte de la librería estándar). El notebook requiere `matplotlib` y `numpy`, disponibles por defecto en Google Colab.

---

## 📊 Resultados obtenidos (seed=42)

| Categoría | Jugador 1 | Jugador 2 |
|---|---|---|
| Ases | 4 | 2 |
| Doses | 4 | 6 |
| Treses | 6 | 3 |
| Cuatros | 4 | 8 |
| Cincos | 5 | 10 |
| Seises | 12 | 12 |
| Subtotal superior | 35 | 41 |
| Bono (+35 si ≥ 63) | 0 | 0 |
| Trío | 24 | 14 |
| Cuádruple | 13 | 0 |
| Full House | 25 | 25 |
| Escalera Corta | 30 | 30 |
| Escalera Larga | 40 | 40 |
| Yahtzee | 0 | 50 |
| Azar | 27 | 22 |
| **TOTAL** | **194** | **222** |

**🏆 Ganador: Jugador 2 — 222 puntos** (diferencia de 28 puntos)

El momento decisivo fue el turno 6 de Jugador_2: el motor detectó cuatro seises y decidió guardarlos. En el siguiente lanzamiento completó los cinco seises, logrando un **Yahtzee de 50 puntos**.

---

## 📐 Parámetros de la simulación

| Parámetro | Valor | Justificación |
|---|---|---|
| N simulaciones (retención) | 6.000 | Balance precisión / velocidad |
| N simulaciones (categoría) | 800 | Suficiente para 13 categorías |
| N simulaciones (Markov) | 3.000 | Estimación de transiciones |
| Semilla aleatoria | 42 | Reproducibilidad académica |
| Distribución de probabilidad | Uniforme U(1,6) | P(X=k) = 1/6 para k ∈ {1..6} |

---

## 📝 Contenido del informe

El documento `Londoño_Santiago_Montecarlo.docx` contiene:

1. **Introducción** — contexto del problema, qué es Montecarlo y su relación con el Yahtzee
2. **Metodología** — distribución de probabilidad, diseño del sistema, cómo funciona el motor de decisiones
3. **Resultados** — marcadores finales, estadísticas por jugador, análisis de transiciones Markov, gráficos
4. **Conclusiones** — reflexiones sobre el método, limitaciones y mejoras futuras
5. **Referencias** bibliográficas

---

## 🔗 Referencias

- Rubinstein, R. Y., & Kroese, D. P. (2016). *Simulation and the Monte Carlo Method* (3ra ed.). Wiley.
- Python Software Foundation. (2024). *random — Generate pseudo-random numbers*. https://docs.python.org/3/library/random.html
- Solitaired. (2024). *Yahtzee en línea*. https://solitaired.com/yahtzee

---

*Actividad Didáctica 2 — Simulación · Institución Universitaria Digital de Antioquia · 2026*
