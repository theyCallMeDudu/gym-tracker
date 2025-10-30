# GymTrack — Product Requirements Document (PRD)

## 1. Visão Geral
- **Produto**: GymTrack — app pessoal para controle de treinos, dieta e evolução corporal, com relatórios semanais.
- **Público-alvo**: praticantes de musculação que desejam registrar treino/dieta e acompanhar progresso (inicialmente uso pessoal, com visão de produto).
- **Proposta de valor**: substituir planilhas dispersas por um fluxo simples diário (tela "Hoje"), com registro rápido e insights semanais.
- **Plataformas**: Web (Desktop/Mobile Web). Futuro: PWA nativa.

## 2. Objetivos e Não-Objetivos
- Objetivos
  - Entregar fluxo mínimo diário funcional em 2-3 semanas.
  - Facilitar registro de treino com séries e carga por membro.
  - Registrar dieta do dia com totais e metas.
  - Visualizar métricas corporais e tendências semanais.
- Não-Objetivos (MVP)
  - Periodização avançada, recomendações automáticas, integrações com wearables.
  - Social, gamificação, notificações push.
  - Apps nativos.

## 3. Personas (Resumo)
- "Atleta Solo": treina 4-6x/semana, deseja histórico, volume por grupo muscular e estimativa de 1RM.
- "Controle de Dieta": quer metas de proteína/kcal e ver aderência semanal.
- "Acompanhamento Físico": quer registrar peso, medidas e fotos, e ver tendência.

## 4. Principais Casos de Uso
- Iniciar/retomar sessão de treino do dia, adicionar exercícios e séries (com peso esquerdo/direito quando aplicável), finalizar sessão com observações e RPE médio.
- Registrar refeições do dia, somar macros e comparar com metas diárias.
- Registrar métricas básicas (peso, cintura etc.) e fazer upload simples de fotos (link/local path no MVP).
- Visualizar relatório semanal com volume por grupo muscular, 1RM estimado e aderência de dieta.

## 5. Escopo Funcional (MVP)
- Autenticação: login/logout, perfil básico (Bearer via Sanctum).
- Treinos:
  - Cadastro base de exercícios (nome, grupo muscular, variação, unilateral).
  - Sessão do dia: criar (start), finalizar (finish), observações, RPE médio.
  - Exercícios da sessão: ordenar, suportar supersets simples (A/B via `superset_group`).
  - Séries: reps, peso esquerdo/direito, tempo, RPE por série.
- Dieta:
  - Cadastro de alimentos (macro por 100g) e opcional `grams_per_unit`.
  - Refeições do dia por slot (pré, café, almoço, lanche, janta, ceia). Totais em tempo real.
- Métricas:
  - Registro de peso, medidas (opcional por campo), BF estimado manual e fotos (path simples).
- Relatórios (Python):
  - Geração semanal de resumo (volume, 1RM estimado, aderência kcal/proteína), salvo como JSON no backend.

## 6. Requisitos Não-Funcionais
- Arquitetura: Angular SPA + Laravel API + MySQL; Python para analytics e import.
- Segurança: Auth via Sanctum, CORS configurado; rate limiting básico.
- Performance: consultas com Eloquent e eager loading; índices críticos em tabelas de sessão/sets.
- Qualidade: padrões de DTO, validação server-side, respostas JSON com data/meta/links.

## 7. Modelo de Dados (Núcleo)
- Usuários
  - `users`: id, name, email, password_hash, settings(json)
- Treinos
  - `exercises`: id, name, muscle_group, variation, unilateral(bool)
  - `workout_sessions`: id, user_id, date, started_at, finished_at, notes, rpe_avg
  - `workout_exercises`: id, session_id, exercise_id, `order`, superset_group(nullable)
  - `workout_sets`: id, workout_exercise_id, set_index, reps, weight_left, weight_right, tempo, rpe
- Dieta
  - `foods`: id, name, unit("g"|"unit"), kcal_per_unit, protein_per_unit, carbs_per_unit, fat_per_unit, grams_per_unit(nullable)
  - `daily_meals`: id, user_id, date, meal_slot
  - `daily_meal_items`: id, daily_meal_id, food_id, quantity
- Métricas
  - `body_metrics`: id, user_id, date, weight, chest, waist, hip, bf_estimate
  - `progress_photos`: id, user_id, date, path_front, path_side, path_back
- Relatórios
  - `weekly_reports`: id, user_id, week_start, payload(json), created_at
- Índices propostos
  - `workout_sessions(user_id, date)`; `workout_exercises(session_id, order)`; `daily_meals(user_id, date, meal_slot)`.

## 8. API (Rascunho de Contratos)
- Auth
  - `POST /auth/login` → { data: { token, user }, meta }
  - `POST /auth/logout` → 204
  - `GET /me` → { data: user }
- Exercises
  - `GET /exercises?query=&muscle_group=` → { data: [exercise], meta }
  - `POST /exercises` → { data: exercise }
- Sessions
  - `POST /sessions` (start hoje) → { data: session }
  - `PATCH /sessions/{id}` (finish/notes) → { data: session }
  - `GET /sessions?date=YYYY-MM-DD` → { data: [session] }
- Session Exercises/Sets
  - `POST /sessions/{id}/exercises` → { data: workout_exercise }
  - `POST /session-exercises/{id}/sets` → { data: set }
  - `PATCH /sets/{id}` → { data: set }
- Foods
  - `GET /foods?query=` → { data: [food], meta }
  - `POST /foods` → { data: food }
- Meals
  - `GET /meals?date=YYYY-MM-DD` → { data: { meals: [...], totals: { kcal, protein, carbs, fat } } }
  - `POST /meals` → { data: daily_meal }
  - `POST /meals/{id}/items` → { data: daily_meal, totals }
- Metrics
  - `GET /metrics?from=&to=` → { data: [metric] }
  - `POST /metrics` → { data: metric }
- Reports
  - `GET /reports/weekly?week=YYYY-Www` → { data: report }
  - `POST /reports/weekly` (consumido pelo Python) → { data: report }

## 9. UX — Telas (Esboços)
- Today (/today)
```
┌──────────────────────────────────────────────────────────────────┐
│ Hoje — (data)                                                    │
├───────────────────────┬───────────────────────────┬──────────────┤
│ Treino do dia         │ Dieta do dia              │ Métricas     │
│ [ Iniciar sessão ]    │ Prot: 80/150g             │ Peso: 66.0   │
│ Último: Peito/Tríceps │ Kcal: 1200/1800           │ BF: 14%      │
│ Volume semanal: ↑     │ [ + Adicionar refeição ]  │ [ + Registrar]│
│ [ Ir para Sessão ]    │ [ Ver detalhes ]          │ [ Histórico ]│
├───────────────────────┴───────────────────────────┴──────────────┤
│ Relatório semanal: ↑ Volume Peito | Aderência Prot: 72%          │
└──────────────────────────────────────────────────────────────────┘
```
- Sessão de Treino, Dieta do Dia, Métricas e Relatórios seguem esboços do plano base.

## 10. Estratégia de Import (Planilha)
- Abas esperadas: "Exercícios", "Alimentos"; opcional "Plano Semanal".
- Pipeline:
  - Python `importers/spreadsheet_import.py` (XLSX → JSON normalizado).
  - Laravel `POST /import/seed` valida e popula `exercises` e `foods`.
  - Log e relatório de import (sucesso, rejeitados, motivos).

## 11. Analytics & Relatórios (Python)
- Weekly Report (`analytics/weekly_report.py`):
  - Volume por grupo muscular (sets × reps × carga média).
  - 1RM estimado (Epley) por exercícios-chave.
  - Aderência calórica e de proteína vs metas.
  - Tendências semana a semana (↑ ↓ →).
  - Output: JSON postado em `weekly_reports` no backend.

## 12. Roadmap de Entregas (MVP)
- v0.1: Auth + rota "/today" estática com placeholders.
- v0.2: Sessões (start/finish) + listar sessão do dia.
- v0.3: Exercícios e séries (com peso esquerdo/direito) na sessão.
- v0.4: Alimentos + refeições do dia com totais e metas.
- v0.5: Métricas básicas (peso/cintura) + histórico curto.
- v0.6: Relatório semanal (Python) + endpoint e card em "Hoje".

## 13. Critérios de Aceite (exemplos)
- "Iniciar sessão": cria `workout_sessions` de hoje (idempotente) e botão muda para "Ir para Sessão".
- "Adicionar série": valida campos, salva `workout_sets` e atualiza totais do exercício.
- "Adicionar refeição": ao salvar item, API retorna totais do dia recalculados, UI reflete imediatamente.
- "Registrar métrica": persistência + atualização do histórico em tela.
- "Relatório semanal": JSON gerado e exibido com setas de tendência.

## 14. Sucesso e Métricas
- Adoção: nº de dias com pelo menos 1 registro (treino ou refeição) por semana.
- Engajamento: tempo para registrar uma série (< 10s) e uma refeição (< 20s).
- Retenção: % de semanas ativas em 4 semanas.

## 15. Riscos e Mitigações
- Abandono por complexidade: foco em tela "Hoje" e fluxo rápido (botões diretos, defaults).
- Dados ruins na planilha: validação forte no import, logs e correção incremental.
- Escopo inflando: releases quinzenais, backlog claro de não-objetivos.

## 16. Itens em Aberto
- Definir metas padrão (kcal/proteína) por usuário vs globais.
- Lista final de exercícios base (seed) e grupos musculares.
- Confirmar layout final da planilha para import (nomes de colunas/abas).

## 17. Anexos e Referências
- Plano MVP: `gym_track_plano_mvp_laravel_angular_my_sql_python.md`.
- Planilha base: `Dudu do zap zap.xlsx`.
