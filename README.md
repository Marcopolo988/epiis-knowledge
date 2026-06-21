# Chatbot Académico EPIIS — Repositorio de Conocimiento

**Repositorio:** https://github.com/Marcopolo988/epiis-knowledge

Repositorio de conocimientos y código del chatbot de atención académica de la **Escuela Profesional de Ingeniería Informática y de Sistemas (EPIIS)** de la Universidad Nacional de San Antonio Abad del Cusco (UNSAAC).

Forma parte del Proyecto Semestral del curso **IF651 Inteligencia Artificial** (2026-1).

---

## Estructura del repositorio

```
repo-epiis/
├── README.md
├── src/                            # Código fuente del chatbot
│   ├── __init__.py
│   ├── chatbot.py                  # Orquestador principal (ChatbotEPIIS)
│   ├── intent_classifier.py        # Clasificador por keywords + Jaccard
│   ├── knowledge_loader.py         # Carga de archivos JSON
│   ├── response_generator.py       # Generador de respuestas
│   └── utils.py                    # Normalización de texto
├── data/                           # Base de conocimiento (respuestas QA)
│   ├── tutorias.json               # Tutoría académica (TUT/TTUT)
│   ├── malla_semestralizada.json   # Cursos por semestre (CUR)
│   ├── plan_estudios_resumen.json  # Especialidades y áreas (ESP)
│   ├── practicas.json              # Prácticas Pre-Profesionales (PPP)
│   ├── bienestar.json              # Bienestar universitario (BIE)
│   ├── movilidad.json              # Movilidad estudiantil (MOV)
│   ├── matricula.json              # Matrícula (MAT)
│   ├── titulacion.json             # Titulación y grado (TIT)
│   └── servicios_academicos.json   # Servicios académicos (SER)
├── knowledge_base/                 # Configuración del clasificador
│   ├── intents.json                # Definición de intenciones
│   └── keywords.json               # Keywords y trigger phrases por intent
├── corpus/
│   └── corpus_consultas.json       # Corpus de evaluación (84 consultas etiquetadas)
├── notebooks/
│   └── chatbot_epiis.ipynb         # Demo interactiva (Google Colab)
├── tests/
│   ├── test_chatbot.py             # Tests del clasificador y respuestas (16 tests)
│   └── test_knowledge_loader.py    # Tests de carga de datos (7 tests)
├── docs/
│   ├── arquitectura.md             # Diagrama de flujo y descripción de módulos
│   └── modelo_datos.md             # Esquema de los archivos JSON
└── sources/
    └── README.md                   # Trazabilidad de fuentes (PDFs utilizados)
```

---

## Arquitectura del chatbot

El chatbot utiliza un clasificador **basado en keywords y similitud Jaccard**, sin dependencias externas de NLP ni modelos de lenguaje. El flujo es:

```
Pregunta del usuario
      │
      ▼
  normalize_text()          # minúsculas + sin tildes + sin puntuación
      │
      ▼
  IntentClassifier          # score = 0.65 × keyword_score + 0.35 × trigger_score
      │
      ├─ score ≥ 0.35  →  ResponseGenerator → respuesta con fuente
      └─ score < 0.35  →  mensaje de fallback
```

Prefijo de intent → archivo de datos:

| Prefijo | Archivo |
|---------|---------|
| TUT / TTUT | tutorias.json |
| CUR | malla_semestralizada.json |
| ESP | plan_estudios_resumen.json |
| PPP | practicas.json |
| BIE | bienestar.json |
| MOV | movilidad.json |
| MAT | matricula.json |
| TIT | titulacion.json |
| SER | servicios_academicos.json |

---

## Cómo ejecutarlo

### Google Colab (recomendado)

1. Abre [notebooks/chatbot_epiis.ipynb](notebooks/chatbot_epiis.ipynb) en Google Colab
2. Ejecuta la celda de configuración — clona este repositorio automáticamente
3. Ejecuta las celdas siguientes para importar el chatbot, probarlo y evaluar el corpus

### Ejecución local

```python
import sys
sys.path.insert(0, '/ruta/a/repo-epiis')

from src.chatbot import ChatbotEPIIS

bot = ChatbotEPIIS('/ruta/a/repo-epiis')
print(bot.ask('¿Cuándo es la matrícula?'))
```

### Tests

```bash
python -m pytest tests/ -v
```

---

## Corpus de evaluación

El archivo `corpus/corpus_consultas.json` contiene **84 consultas etiquetadas** con su `intent_esperado`, distribuidas sobre todas las categorías temáticas. Sirve como corpus de evaluación para medir la accuracy del clasificador de forma automatizada:

```
Accuracy: 84/84 = 100.0%
```

La celda 5 del notebook ejecuta la evaluación completa y muestra los errores en detalle.

---

## Datos institucionales

- **Universidad:** Universidad Nacional de San Antonio Abad del Cusco (UNSAAC)
- **Facultad:** Ingeniería Eléctrica, Electrónica, Informática y Mecánica (FIEEIM)
- **Escuela:** Ingeniería Informática y de Sistemas (EPIIS)
- **Plan Curricular:** 2024 — vigente desde Año Académico 2025
- **Web oficial:** https://in.unsaac.edu.pe/
- **Repositorio:** https://github.com/Marcopolo988/epiis-knowledge
