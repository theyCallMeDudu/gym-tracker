# GymTrack — Esboços de Telas (Wireframes ASCII)

Nota: Esboços conceituais para guiar UX/flows no MVP. Componentização pensada para Angular (cards, lists, modals). 

## 1) Login
```
┌─────────────────────────────┐
│     GymTrack — Login        │
├─────────────────────────────┤
│ Email:    [              ]  │
│ Senha:    [              ]  │
│ [ Entrar ]     [ Criar conta ]
│                                  
│ Esqueceu a senha? [ Recuperar ]  
└─────────────────────────────┘
```

## 2) Shell (Layout) + Navegação
```
┌───────────────────────────────────────────────────────────────┐
│ GymTrack                                   (User • Sair)      │
├─────────────────┬─────────────────────────────────────────────┤
│ Início          │ [ Conteúdo da página atual ]                │
│ Hoje            │                                             │
│ Treinos         │                                             │
│ Dieta           │                                             │
│ Métricas        │                                             │
│ Relatórios      │                                             │
└─────────────────┴─────────────────────────────────────────────┘
```

## 3) Hoje (/today)
```
┌─────────────────────────────────────────────────────────────────────────────┐
│ Hoje — Qua, 29/10                                                           │
├───────────────────────┬───────────────────────────┬─────────────────────────┤
│ Treino do dia         │ Dieta do dia              │ Métricas rápidas       │
│ [ Iniciar sessão ]    │ Prot: 80/150g             │ Peso: 66.0             │
│ Último: Peito/Tríceps │ Kcal: 1200/1800           │ Cintura: 79            │
│ Volume semanal: ↑     │ [ + Adicionar refeição ]  │ BF: 14%                │
│ [ Ir para Sessão ]    │ [ Ver detalhes ]          │ [ + Registrar ]        │
├───────────────────────┴───────────────────────────┴─────────────────────────┤
│ Relatório semanal: ↑ Volume Peito | Aderência Prot: 72%  [ Ver relatório ]  │
└─────────────────────────────────────────────────────────────────────────────┘
```

## 4) Sessão de Treino (/workouts/session/:id)
```
Peito/Tríceps — Sessão #123 (em andamento)          [ Finalizar sessão ]
Notas: [                                                                         ]
RPE médio: [ 8.0 ]

[ + Adicionar exercício ]   [ Reordenar exercícios ]

▸ Supino reto (halteres)          [ A/B Superset ]  [ Remover ]
  Série 1: Reps [12]  Esq [17.5]  Dir [17.5]  RPE [8]   [ Salvar ] [ X ]
  Série 2: Reps [10]  Esq [20.0]  Dir [20.0]  RPE [9]   [ Salvar ] [ X ]
  [ + Adicionar série ]  [ Duplicar última ]  Totais: Vol 1.9k

▸ Crucifixo inclinado             [ A/B Superset ]  [ Remover ]
  (sem séries)
```

### 4.1) Modal — Adicionar Exercício
```
┌──────────────────────────────────────────────┐
│ Adicionar Exercício                         │
├──────────────────────────────────────────────┤
│ Buscar: [ supi       ]  Grupo: [ Peito v ]  │
│                                              │
│ Resultados:                                  │
│ - Supino reto (halteres)         [Adicionar] │
│ - Supino inclinado (halteres)    [Adicionar] │
│ - Crucifixo inclinado            [Adicionar] │
│                                              │
│ [ Fechar ]                                   │
└──────────────────────────────────────────────┘
```

### 4.2) Modal — Finalizar Sessão
```
┌─────────────────────────────────────────────────────────────┐
│ Finalizar Sessão                                            │
├─────────────────────────────────────────────────────────────┤
│ Duração: 01:02:33                                           │
│ RPE médio: [ 8.5 ]                                          │
│ Observações:                                                │
│ [ As últimas séries de supino ficaram pesadas...         ]  │
│                                                             │
│ [ Cancelar ]                          [ Salvar e finalizar ]│
└─────────────────────────────────────────────────────────────┘
```

## 5) Histórico de Treinos (/workouts)
```
┌──────────────────────────────────────────────────────────────┐
│ Histórico de Treinos                                         │
├─────────────┬───────────────┬───────────────────────────────┤
│ Data        │ Título        │ Destaques                    │
├─────────────┼───────────────┼───────────────────────────────┤
│ 29/10       │ Peito/Tríceps │ Supino 20kg x10; Volume ↑    │
│ 27/10       │ Costas/Bíceps │ Remada 60kg x8; Volume →     │
│ 25/10       │ Pernas        │ Agachamento 100kg x5; Vol ↓  │
└─────────────┴───────────────┴───────────────────────────────┘
[ Ver sessão ]  [ Duplicar como plano ]  [ Exportar CSV ]
```

## 6) Dieta do Dia (/diet/day?date=)
```
Data: [ 29/10/2025 ]      Totais: Prot 80/150g  |  Kcal 1200/1800
[ + Adicionar refeição ]

Pré-treino                                 [ Editar slot ]  [ X ]
  - Banana (2 un) + Aveia (30 g)
    → P 8g  C 60g  G 6g  Kcal 340        [ Editar ] [ X ]

Café da manhã                              [ Editar slot ]  [ X ]
  - Ovos (2) + Pão integral (2)
    → P 20g C 24g G 10g Kcal 300         [ Editar ] [ X ]
```

### 6.1) Modal — Adicionar Item à Refeição
```
┌────────────────────────────────────────────────────────────┐
│ Adicionar item à refeição                                  │
├────────────────────────────────────────────────────────────┤
│ Slot: [ Café da manhã v ]                                  │
│ Buscar alimento: [ banana            ]  Unidade: [ g | un ]│
│                                                            │
│ Resultados:                                                │
│ - Banana  (kcal/100g: 89, prot: 1.1g, carbs: 23g, fat: 0.3g)
│   Quantidade: [ 200 ] g   →  Kcal [178]  Prot [2.2]  Carbs [46]  Fat [0.6]
│   [ Adicionar ]                                            │
│                                                            │
│ [ Cancelar ]                               [ Salvar ]      │
└────────────────────────────────────────────────────────────┘
```

## 7) Catálogo de Alimentos (/diet/foods)
```
┌──────────────────────────────────────────────────────────────┐
│ Alimentos                                                    │
├──────────────────────────────────────────────────────────────┤
│ Buscar: [ frango             ]   [ + Novo alimento ]         │
│                                                              │
│ Lista:                                                       │
│ - Frango cozido (kcal/100g: 165 | P: 31 | C: 0 | G: 3.6)     │
│ - Arroz branco (kcal/100g: 130 | P: 2.4 | C: 28 | G: 0.3)    │
│ - Banana (kcal/100g: 89  | P: 1.1 | C: 23 | G: 0.3)         │
└──────────────────────────────────────────────────────────────┘
```

## 8) Métricas (/metrics)
```
┌──────────────────────────────────────────────────────┐
│ Métricas                                              │
├──────────────────────────────────────────────────────┤
│ Data: [29/10/2025]                                    │
│ Peso [66.0]  Cintura [79]  Peito [96]  BF [%] [14]    │
│ [ Upload fotos ]  [ Salvar ]                          │
├──────────────────────────────────────────────────────┤
│ Histórico (últimos 7 registros)                       │
│ 28/10 - 66.1 | 27/10 - 66.3 | 26/10 - 66.0 | ...      │
└──────────────────────────────────────────────────────┘
```

## 9) Relatórios Semanais (/reports/weekly)
```
┌──────────────────────────────────────────────────────────────────┐
│ Relatório Semanal — Semana 2025-W44                               │
├────────────────────────────┬──────────────────────────────────────┤
│ Volume por grupo           │ 1RM estimado (Epley)                │
│ Peito ↑ 12.3k              │ Supino reto: 95 kg (+2)             │
│ Costas → 10.1k             │ Agachamento: 130 kg (-1)            │
│ Pernas ↓ 8.9k              │ Terra: 160 kg (+0)                  │
├────────────────────────────┴──────────────────────────────────────┤
│ Aderência dieta: Kcal 86%  |  Proteína 74%                        │
│ [ Baixar JSON ]  [ Ver detalhes ]                                 │
└──────────────────────────────────────────────────────────────────┘
```

## 10) Erros e Variações de Estado
```
[ Treino do dia ]
- Sem sessão iniciada: mostrar botão "Iniciar sessão".
- Sessão em andamento: mostrar botão "Ir para Sessão".
- Sessão finalizada hoje: mostrar resumo e opção de reabrir (MVP: somente leitura).

[ Dieta do dia ]
- Sem itens: CTA proeminente para adicionar refeição.
- Totais calculados e atualizados após salvar item/remoção.

[ Métricas ]
- Campos opcionais; salvar permite parciais.
```

## 11) Componentes Reutilizáveis (Angular)
- Cards: `StatCard`, `ListCard`, `FormCard`.
- Pickers: `ExercisePicker`, `FoodPicker`.
- Linhas: `SetRow`, `MealItemRow`.
- Dialogs: `ConfirmDialog`, `FinishSessionDialog`.

## 12) Navegação e Fluxos Rápidos
- Hoje → (Iniciar sessão) → Sessão de Treino.
- Hoje → (Adicionar refeição) → Dieta do Dia (modal de item).
- Hoje → (Registrar métrica) → Métricas (salva e retorna).
- Sessão finalizada → Relatórios (card com link na tela Hoje).
