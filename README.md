# Fuoco

Aplicativo de produtividade feito em Expo/React Native com IA nativa (on-device) via `llama.rn`. O foco do Fuoco e ajudar o usuario a organizar rotina, tarefas, despesas e treinos com um assistente local que entende linguagem natural e automatiza registros.

## O que o Fuoco faz
- Assistente de produtividade dentro do app (chat) para organizar tarefas e despesas.
- Agenda com tarefas, lembretes e notificacoes locais.
- Controle de despesas e ganhos com categorizacao.
- Treinos, rotinas recorrentes e timer para foco.
- Sistema de progresso/XP e estatisticas do usuario.
- PIN com biometria (Face ID/Touch ID quando disponivel).

## Principais telas e fluxos
- **Welcome/Onboarding**: entrada do usuario e escolha de avatar.
- **Pin**: bloqueio do app com SecureStore + biometria.
- **Tabs principais**: Chat, Social, Agenda, Despesas e Treinos.
- **Rotinas/Timer/Settings**: acesso via stack para configuracoes e utilitarios.

## IA nativa com `llama.rn`
O Fuoco roda IA **localmente** usando `llama.rn`, sem depender de servidor para gerar respostas.

- **Modelo**: `google_gemma-3-1b-it` (GGUF `Q4_K_M`) baixado no primeiro uso.
- **Armazenamento**: `DocumentDirectory/models` via `react-native-fs`.
- **Bootstrap**: download com progresso e cache em `AsyncStorage`.
- **Contexto**: `n_ctx: 2048`, `n_gpu_layers: 99` (efeito no iOS).
- **Prompts**: sistema com avatars e estilo de resposta (arquivo `llm/systemPrompt.ts`).

## NLP e automacoes
O chat identifica intentos e cria registros automaticamente:
- **Tarefas**: extraidas por data/hora (chrono-node + compromise-dates).
- **Despesas**: extraidas por valor e contexto financeiro (compromise + regex).
- **Roteamento**: regras locais para decidir entre task, expense ou resposta geral.

## Armazenamento e dados
Tudo e salvo localmente no dispositivo.

- **SQLite (expo-sqlite)** com WAL e migracoes.
- **SQLCipher** habilitado no iOS (via plugin).
- **Tabelas principais**: `user`, `tasks`, `routine_tasks`, `expenses`, `workouts`, `notes`, `goals`, `category`.
- **AsyncStorage**: historico do chat e metadata do modelo.
- **SecureStore**: `user_id`, `user_name`, `user_pin`, avatar.

## Notificacoes
Tarefas agendadas recebem:
- aviso no horario da tarefa
- aviso 1 hora antes (se aplicavel)

## Stack tecnico
- **Expo 53** + **React Native 0.79** + **React 19**
- **Navegacao**: React Navigation (stack + bottom tabs)
- **UI**: NativeWind + Styled Components + Moti + Reanimated
- **IA local**: `llama.rn`
- **Data**: SQLite + AsyncStorage + SecureStore
- **Utilitarios**: date-fns, chrono-node, compromise

## Estrutura do projeto
- `App.tsx`: bootstrap do app, fontes, DB, notificacoes e LLM.
- `components/`: telas e componentes de UI.
- `hooks/`: regras de negocio (tasks, expenses, stats, auth).
- `database/`: setup, pragmas e migracoes do SQLite.
- `llm/`: bootstrap do modelo e prompts de sistema.
- `nlp/`: parsers de intent, data e despesas.
- `tabs/`: navegacao principal.

## Scripts
```bash
npm run start   # Expo dev server
npm run ios     # build nativo iOS
npm run android # build nativo Android
npm run web     # Expo Web
npm run lint
npm run format
```

## Requisitos locais
- Node.js LTS
- Expo CLI
- Xcode (iOS) e/ou Android Studio (Android)

## Observacoes
- O primeiro uso baixa o modelo local; pode levar alguns minutos dependendo da rede.
- O app nao depende de backend para o fluxo principal; dados ficam no dispositivo.

