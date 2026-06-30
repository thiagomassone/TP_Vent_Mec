# TP Desafío Especial — Ventilación Mecánica (16.09)

**Autor:** Thiago Massone (legajo 60035)
**Curso:** Ventilación Mecánica - ITBA

## Consigna 4

Cálculo de la resistencia y la elastancia (compliance) del sistema respiratorio por medio de una regresión lineal, usando un modelo de 2 elementos, en respiraciones pasivas.

## Resumen

A partir de 4 señales reales de un sistema FluxMed (Flow, Paw, Pes, entre otras, a 256 Hz), se identificó que correspondían a un único registro continuo de ~53 minutos exportado en 4 tramos. Se seleccionó la señal D por presentar la menor variabilidad de presión esofágica (Pes) y mayor cantidad de muestras disponibles.

Dentro de D se identificaron 4 ciclos de respiración pasiva (validados con la curva de Pes como proxy indirecto del esfuerzo muscular del paciente), y sobre cada uno se ajustó el modelo de 2 elementos:

```
Paw(t) = R · F(t) + E · V(t) + PEEP
```

por cuadrados mínimos (`numpy.linalg.lstsq`).

**Resultado final (promedio de los 4 ciclos):**
- Resistencia (R) ≈ 10.5 cmH₂O/(L/s)
- Compliance (C) ≈ 67.3 mL/cmH₂O
- PEEP ≈ 8.9 cmH₂O

Ambos valores se encuentran dentro de rangos fisiológicos normales. El detalle completo del proceso, las figuras de justificación y la discusión de resultados (incluyendo un ciclo atípico identificado y dejado como punto de análisis a futuro) están en el notebook.

## Contenido del repositorio

```
.
├── TP_consigna4.ipynb        # Notebook completo con desarrollo, código y resultados
├── Informe_TP_Consigna4.docx # Informe en formato Word con el mismo contenido
├── signals/                  # Señales originales (FluxMed, 4 archivos .txt)
└── img/                      # Imágenes usadas en el notebook (logo, diagrama del modelo)
```

## Cómo correr la notebook

### Opción 1 — Localmente (Jupyter / VS Code)

```bash
git clone https://github.com/thiagomassone/TP_Vent_Mec.git
cd TP_Vent_Mec
python3 -m venv venv
source venv/bin/activate        # En Windows: venv\Scripts\activate
pip install pandas numpy matplotlib plotly nbformat ipython jupyter
jupyter notebook TP_consigna4.ipynb
```

El notebook asume que `signals/` e `img/` están en el mismo directorio que el `.ipynb` (rutas relativas), por lo que clonar el repo tal cual ya deja todo en su lugar.

### Opción 2 — Google Colab
https://drive.google.com/file/d/1cP-RUoCUsWdLG03Q3c3AUjhEpd9NW6i6/view?usp=sharing


## Estructura del desarrollo (dentro de la notebook)

1. Exploración de las señales disponibles y selección de la más adecuada
2. Selección de tramos de respiración pasiva (justificado con Pes)
3. Cálculo del volumen por integración del flujo
4. Ajuste del modelo por regresión lineal en cada tramo seleccionado
5. Comparación de los resultados obtenidos y promedio de los parámetros
6. Validación del modelo promediado
7. Conclusión, limitaciones y mejoras futuras
