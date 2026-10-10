# Configuración de Linear — Proyecto GCI Credit Default Risk 2026

Configuración operativa creada el 2026-10-09 en el workspace **GCI World** de Linear.
Este documento es el espejo público de esa configuración.

- **Proyecto:** GCI Credit Default Risk 2026
- **URL:** https://linear.app/gci-world/project/gci-credit-default-risk-2026-dbae954dc471
- **Equipo:** GCI World (key `GCI`) · **Asignado:** Mario Marroquín (todas las issues)
- **Sprints:** implementados como *labels* `Sprint N · …` (la API de Linear no permite crear Cycles)
- **Issues públicas espejo:** pestaña Issues de este repo (10 condensadas)

## Milestones

| ID | Nombre | Fecha límite | Definición de éxito |
|---|---|---|---|
| M1 | Baseline en leaderboard | 2026-10-16 | Primera submission subida y brecha CV↔LB medida |
| M2 | Ensemble base | 2026-10-23 | LGBM + XGBoost + CatBoost con blend validado por CV |
| M3 | Requisitos mínimos (red de seguridad) | 2026-11-06 | Pipeline reproducible end-to-end + refs.md + CV ≥ 0.7681 |
| M4 | Métodos extra | 2026-11-15 | Tuning Optuna, stacking/TabPFN, adversarial validation evaluados |
| M5 | Submission final congelada | 2026-11-18 | Modelo final elegido por RSKF 3×5 y CSV subido (2 días de colchón) |

## Issues por sprint

### Sprint 1 · Baseline (9 – 16 oct)
| Issue | Título | Criterios de aceptación |
|---|---|---|
| GCI-5 | Setup del proyecto y descarga de datos | ✅ Hecho: datos oficiales verificados (train 171,202×34 / test 61,500×33 / sample_submission) |
| GCI-6 | Repo público y estructura | Repo con README, refs.md, requirements; visibilidad gestionada por Mario |
| GCI-7 | EDA completo | Reporte con balance de target, nulos, correlaciones, anomalía DAYS_EMPLOYED, cardinalidad de categóricas y drift train↔test |
| GCI-8 | Baseline + primeras features | CV reproducible con ratios, flags de nulos y features de tiempo; comparación contra 0.7581 |
| GCI-9 | Primera submission + lectura del LB | CSV en formato exacto subido a Omnicampus; brecha CV↔LB registrada; si >0.02 → parar y arreglar validación |

### Sprint 2 · Ensemble base (17 – 23 oct)
| Issue | Título | Criterios de aceptación |
|---|---|---|
| GCI-10 | Features EXT_SOURCE avanzadas | Polinómicas grado 2–3 y stats (mean/std/min/max/nan_count) evaluadas por CV |
| GCI-11 | Target encoding OOF | TE con smoothing para ORGANIZATION_TYPE/OCCUPATION_TYPE, calculado out-of-fold (sin leakage) |
| GCI-12 | XGBoost + CatBoost + blend | Ambos modelos en branches propios; blend ponderado por OOF; adoptar solo si supera umbrales |

### Sprint 3 · Requisitos mínimos (24 oct – 6 nov)
| Issue | Título | Criterios de aceptación |
|---|---|---|
| GCI-13 | Reproducibilidad total | `python -m src.train` end-to-end; 2 corridas producen CSV idéntico; seeds en src/config.py |
| GCI-14 | refs.md completo | Toda fuente de código/idea citada con fecha, URL y dónde se usa |
| GCI-15 | Submission M3 | Submission con CV ≥ 0.7681 subida y registrada |
| GCI-16 | Congelar red de seguridad | Tag/branch de M3; documentación de cómo regenerar el CSV |

### Sprint 4 · Métodos extra (7 – 15 nov)
| Issue | Título | Criterios de aceptación |
|---|---|---|
| GCI-17 | Tuning con Optuna | Búsqueda de hiperparámetros con CV; adoptar si +0.002 |
| GCI-18 | Stacking y TabPFN | Meta-modelo logístico sobre OOF; TabPFN solo si corre en CPU y suma ≥0.002 RSKF |
| GCI-19 | Adversarial validation | Clasificador train-vs-test; AUC < 0.6 = aceptable; si no, revisar validación |

### Sprint 5 · Cierre (16 – 18 nov)
| Issue | Título | Criterios de aceptación |
|---|---|---|
| GCI-20 | Selección final por RSKF | Repeated StratifiedKFold 3×5 (seeds 42/43/44); decisión por mean ± std, nunca por public LB |
| GCI-21 | Submission final | CSV final subido a Omnicampus el 18 nov (colchón de 2 días); captura de evidencia |
| GCI-22 | Cierre y documentación | README final, lecciones aprendidas, repo y docs ordenados para evaluación |

## Labels de sprint creados
`Sprint 1 · Baseline` · `Sprint 2 · Ensemble` · `Sprint 3 · Requisitos mínimos` · `Sprint 4 · Métodos extra` · `Sprint 5 · Cierre`

## Notas operativas
- Checkpoints semanales en **jueves/viernes** (la sección de Probability for AI es miércoles 6pm).
- Existe un proyecto duplicado obsoleto en el workspace CUBO_plus que se archiva manualmente.
- Calendario espejo en Google Calendar: *GCI Credit Default* (11 eventos: 5 sprints, 5 hitos, 1 deadline real).
