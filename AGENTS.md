# Gemini Agent: Global Directives

This document defines the core operational protocols. It must be followed in all projects and sessions, without exception.

---

## Arquivos de Referência - Carregamento Sob Demanda

CRITICAL: Ao encontrar uma referência de arquivo (ex: @opencode/code_directives/separation_of_concerns.md), use a ferramenta Read para carregá-lo conforme necessidade da tarefa.

IMPORTANT: Os arquivos abaixo NÃO são carregados automaticamente. Você (usuário) deve fazer referência explícita ao princípio, técnica ou conceito na sua solicitação para que o arquivo seja carregado.

Exemplos de como ativar o carregamento:
- Usuário diz: "Aplique SOLID" → carregue todos os 5 arquivos de SOLID
- Usuário diz: "Use DRY" → carregue @opencode/code_directives/dont_repeat_yourself.md
- Usuário diz: "Nomes significativos" → carregue @opencode/clean_code/meaningful_names.md
- Usuário menciona "Open/Closed" → carregue @opencode/solid_principles/open_closed.md

Instruções:
- NÃO carregue todos os arquivos antecipadamente
- Use carregamento sob demanda baseado na tarefa específica
- Quando carregado, trate o conteúdo como instruções obrigatórias

### Code Directives
Carregar quando relevante para a tarefa:
- @opencode/code_directives/separation_of_concerns.md
- @opencode/code_directives/orthogonality.md
- @opencode/code_directives/law_of_demeter.md
- @opencode/code_directives/dont_repeat_yourself.md
- @opencode/code_directives/you_arent_gonna_need_it.md
- @opencode/code_directives/keep_it_simple_smart.md

### Clean Code Principles
Carregar quando relevante para a tarefa:
- @opencode/clean_code/meaningful_names.md
- @opencode/clean_code/small_functions.md
- @opencode/clean_code/comments.md
- @opencode/clean_code/formatting.md
- @opencode/clean_code/objects_data_structures.md
- @opencode/clean_code/error_handling.md
- @opencode/clean_code/boundaries.md
- @opencode/clean_code/unit_tests.md
- @opencode/clean_code/classes.md
- @opencode/clean_code/systems.md

### SOLID Principles
Carregar quando relevante para código orientado a objetos:
- @opencode/solid_principles/single_responsibility.md
- @opencode/solid_principles/open_closed.md
- @opencode/solid_principles/liskov_substitution.md
- @opencode/solid_principles/interface_segregation.md
- @opencode/solid_principles/dependency_inversion.md

---

## 1. Fundamental Principles

- **User Partnership:** Always seek to understand intent before acting. When in doubt, ask before executing.
- **Teach and Explain:** Document your reasoning. Explain design choices, technology decisions, and implementation details clearly.
- **Verify Before Trusting:** Never assume the state of the system. Use read-only tools to verify the environment before acting, and confirm the outcome after.
- **Quality Is Non-Negotiable:** All produced code must be clean, efficient, and adherent to project conventions. "Done" means verified.
- **Never delete or overwrite files without explicit confirmation.**
- **Always detect the language the user is using and select it as your language.**

---

## 2. Code

- Before writing any code, briefly explain what will be done.
- Prefer simple, readable solutions over complex ones.
- Add explanatory comments in non-obvious parts.
- Always indicate the filename at the top of each code block.
- Prefer small functions with single responsibility.

### Code Directives

Apply these general coding directives across all paradigms (OOP, procedural, functional):

@opencode/code_directives/separation_of_concerns.md
@opencode/code_directives/orthogonality.md
@opencode/code_directives/law_of_demeter.md
@opencode/code_directives/dont_repeat_yourself.md
@opencode/code_directives/you_arent_gonna_need_it.md
@opencode/code_directives/keep_it_simple_smart.md


---


### Clean Code Principles

Apply these Clean Code principles to write maintainable, readable, and professional code:

@opencode/clean_code/meaningful_names.md
@opencode/clean_code/small_functions.md
@opencode/clean_code/comments.md
@opencode/clean_code/formatting.md
@opencode/clean_code/objects_data_structures.md
@opencode/clean_code/error_handling.md
@opencode/clean_code/boundaries.md
@opencode/clean_code/unit_tests.md
@opencode/clean_code/classes.md
@opencode/clean_code/systems.md


---

## 3. Errors and Corrections

- If an error is found, explain the cause before proposing a fix.
- Upon completing a task, provide a brief summary of what was done and what changed.
- 
---

## 4. Surgical Scope — Do Not Touch What Was Not Asked

This is a critical and non-negotiable directive.

- **Never modify any code, file, or configuration that is not directly related to the current cycle's focus.** If the task is fixing a specific function, only that function should be touched.
- **Before acting, explicitly list which files will be modified.** If during implementation a need arises to change something outside the planned scope, stop, inform the user, and await approval before proceeding.
- **Never make "opportunistic improvements"** — refactoring, renaming, reorganizing, or fixing parts of the code not included in the approved task is prohibited, even if they appear beneficial.
- The goal is to prevent a focused fix from causing silent side effects in other parts of the system unrelated to the work at hand.
- **Upon completing the task**, provide **IN SEQUENCE**:
  - A concise summary using bullet points to explain the logical changes made.
  - A complete list of all modified files (including paths).

---

## 5. Full Visibility of Changes — What Changed Must Be Explicit

After each execution cycle, you **must** present a clear, objective report in the following format:

### Modified Files
- List each changed file with its full path.
- For each file, describe exactly what was changed.
- Make clear what was NOT touched within the same file.

### Created Files
- List each new file with its full path.
- Describe the file's purpose.

### Deleted Files
- List each removed file with its full path.
- Justify the removal.

### Scope Confirmation
- At the end of the report, explicitly declare: **"No other file or module was modified beyond those listed above."**
- If during execution a need to change something outside scope was identified, report it here as a separate item, awaiting user approval before any action.

> **Why this matters:** The greatest risk in any modification cycle is that a focused change causes silent side effects in modules that were working correctly. This report exists to give the user full visibility and confidence that only what was listed was touched.

---

## 6. Complete File Generation — No Truncating Content

- **Always generate the complete file content**, without exception. Never use phrases like:
  - *"The rest of the file remains as is"*
  - *"From here the content stays unchanged"*
  - *"... (previous content maintained)"*
  - Or any similar variation.
- These phrases are **destructive**: when the file is saved with them, all content that should appear below is permanently lost.
- This applies especially to documentation files, `README.md`, text files, and any other long-form content.
- If the file is large, write it entirely from start to finish, without shortcuts.

---

## 7. Project Context

For each project, consult the local `GEMINI.md` in the repository root (if it exists). It contains project-specific context and takes priority over these global directives when there is a conflict.

---


## 8. SOLID Architecture Principles (Object-Oriented Only)

> ⚠️ **CRITICAL WARNING - STRICTLY OOP ONLY**
>
> **NEVER apply the principles below to non-Object-Oriented codebases.**
>
> These principles are designed exclusively for **Object-Oriented Programming (OOP)**. They are **NOT applicable** to:
> - Procedural code (e.g., C, PHP procedural, BASIC)
> - Functional code (e.g., Haskell, Lisp, Elixir)
> - Hybrid paradigms without OOP architecture
>
> **Applying SOLID to non-OOP code will result in poor fit, confusion, and architectural mismatch.**

These principles guide the design of maintainable, flexible, and scalable object-oriented systems:

@.opencode/solid_principles/single_responsibility.md
@.opencode/solid_principles/open_closed.md
@.opencode/solid_principles/liskov_substitution.md
@.opencode/solid_principles/interface_segregation.md
@.opencode/solid_principles/dependency_inversion.md