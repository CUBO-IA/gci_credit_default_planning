# Registro de decisiones — GCI Credit Default Risk 2026

Formato: fecha · decisión · contexto/motivo. Fuente: acuerdos de Mario Marroquín con su asistente (Fluso).

## 2026-10-09 — Estrategia y gobierno del proyecto

1. **Los datos se descargan del Drive oficial al sandbox de trabajo** (no al repo; `data/raw` no se versiona).
   Motivo: reproducibilidad sin violar tamaño del repo ni redistribuir datos de la competencia.
2. **La visibilidad del repo de código la gestiona Mario manualmente.**
   (Este repo de planificación nace público por decisión explícita del mismo día.)
3. **Regla de freno CV↔LB:** si la brecha |CV − leaderboard| > 0.02 AUC, se **detiene la iteración** y se corrige primero la validación (domain shift o leakage) antes de seguir.
   Motivo: evitar sobreajustar al public LB, que es un subconjunto del test.
4. **Exprimir cada centésima de AUC:** guardar cada modelo (OOF, params, seed) y hacer **un branch por experimento**; `main` = mejor verificado por CV.
   Umbrales de adopción a `main`: +0.003 features · +0.002 tuning · +0.001 blend/stacking.
5. **Definición de M3 (requisitos mínimos, 6 nov):** pipeline end-to-end ejecutable con `python -m src.train`, seeds fijas (2 corridas = CSV idéntico), `refs.md` con citas completas, y una submission con CV ≥ 0.7681 (baseline 0.7581 + 0.01). Es la red de seguridad antes de métodos avanzados.
6. **La submission final se elige por Repeated StratifiedKFold (3×5, seeds 42/43/44), NUNCA por el public leaderboard.**
   Motivo: solo cuenta la última submission; decidir por el LB público sobreajusta al subset visible.

## 2026-10-09 — Metodología técnica (post-análisis de datos)

7. **Estrategia single-table:** la competencia usa solo la tabla application-level; el plan original de 7 tablas del Kaggle 2018 no aplica. Baseline = LightGBM + features intra-tabla + StratifiedKFold-5. Ruta de ensemble: LGBM + XGBoost + CatBoost (+ TabPFN experimental en M4).
8. **Preprocesamiento:** no imputar `EXT_SOURCE_*` (el faltante predice: 8.9% vs 7.7% de default); flags `*_ISNA`; flag + NaN para la anomalía `DAYS_EMPLOYED = 365243` (18% de las filas); target encoding **out-of-fold** con smoothing para `ORGANIZATION_TYPE`/`OCCUPATION_TYPE`; sin escalado (boosting puro).
9. **Gestión en tres frentes sincronizados:** Linear (operativo, 18 issues GCI-5…GCI-22 + milestones M1–M5 + labels de sprint), GitHub (issues públicos espejo + código), Google Calendar (sprints y hitos). Los checkpoints caen jueves/viernes para no chocar con la sección de Probability for AI (miércoles 6pm).

## 2026-10-09 — Resultados de la primera noche de experimentos

10. **Ningún experimento inicial supera el umbral de adopción; `main` queda intacto.**
    v1 ratios+flags+tiempo: CV 0.75827 (+0.00017) · v2 polinómicas EXT + TE-OOF: 0.75804 (−0.00006) · v3 tuning: 0.75888 (+0.00078, mejor).
    Conclusión registrada: **el tuning mueve más que las features** → Optuna sube de prioridad (Sprint 4, se adelanta exploración si hay tiempo en Sprint 2).
11. **Primera submission al LB con el modelo v3** (CV 0.75888) para calibrar la brecha CV↔LB (regla 3).
