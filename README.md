# Fuoco

A productivity app built with Expo/React Native featuring native (on-device) AI via `llama.rn`. Fuoco’s goal is to help users organize their routines, tasks, expenses, and workouts through a local assistant that understands natural language and automates record-keeping.

## What Fuoco Does
- In-app productivity assistant (chat) to organize tasks and expenses.
- Calendar with tasks, reminders, and local notifications.
- Expense and income tracking with categorization.
- Workouts, recurring routines, and a focus timer.
- User progress/XP system and personal statistics.
- PIN lock with biometrics (Face ID / Touch ID when available).

## Main Screens and Flows
- **Welcome / Onboarding**: user entry and avatar selection.
- **PIN**: app lock using SecureStore + biometrics.
- **Main Tabs**: Chat, Social, Calendar, Expenses, and Workouts.
- **Routines / Timer / Settings**: accessed via stack navigation for configuration and utilities.

## On-device AI with `llama.rn`
Fuoco runs AI **locally** using `llama.rn`, without relying on a server to generate responses.

- **Model**: `google_gemma-3-1b-it` (GGUF `Q4_K_M`) downloaded on first use.
- **Storage**: `DocumentDirectory/models` via `react-native-fs`.
- **Bootstrap**: download with progress tracking and caching in `AsyncStorage`.
- **Context**: `n_ctx: 2048`, `n_gpu_layers: 99` (effective on iOS).
- **Prompts**: system prompts with avatars and response style (`llm/systemPrompt.ts`).

## NLP and Automations
The chat identifies intents and automatically creates records:
- **Tasks**: extracted with date/time (chrono-node + compromise-dates).
- **Expenses**: extracted by amount and financial context (compromise + regex).
- **Routing**: local rules decide between task, expense, or general response.

## Storage and Data
All data is stored locally on the device.

- **SQLite (expo-sqlite)** with WAL and migrations.
- **SQLCipher** enabled on iOS (via plugin).
- **Main tables**: `user`, `tasks`, `routine_tasks`, `expenses`, `workouts`, `notes`, `goals`, `category`.
- **AsyncStorage**: chat history and model metadata.
- **SecureStore**: `user_id`, `user_name`, `user_pin`, avatar.

## Notifications
Scheduled tasks receive:
- a notification at the task time
- a notification 1 hour before (when applicable)

## Tech Stack
- **Expo 53** + **React Native 0.79** + **React 19**
- **Navigation**: React Navigation (stack + bottom tabs)
- **UI**: NativeWind + Styled Components + Moti + Reanimated
- **Local AI**: `llama.rn`
- **Data**: SQLite + AsyncStorage + SecureStore
- **Utilities**: date-fns, chrono-node, compromise

## Project Structure
- `App.tsx`: app bootstrap, fonts, DB, notifications, and LLM.
- `components/`: screens and UI components.
- `hooks/`: business logic (tasks, expenses, stats, auth).
- `database/`: SQLite setup, pragmas, and migrations.
- `llm/`: model bootstrap and system prompts.
- `nlp/`: intent, date, and expense parsers.
- `tabs/`: main navigation.

## Scripts
```bash
npm run start   # Expo dev server
npm run ios     # native iOS build
npm run android # native Android build
npm run web     # Expo Web
npm run lint
npm run format
