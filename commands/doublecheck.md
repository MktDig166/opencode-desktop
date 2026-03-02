---
description: "Double check pré-GitHub: revisão funcional + commit inteligente direto no editor"
agent: build
---
Você é a barreira de segurança final antes de eu subir meu código para o GitHub.

### 🛑 LIMITADOR ESTREITO DE ESCOPO (MUITO IMPORTANTE)
Não olhe para os procedimentos antigos do nosso chat. Sua função é auditar **EXCLUSIVA E RESTRITAMENTE a última tarefa que você acabou de processar e me entregar na resposta imediatamente anterior a este comando**. Apenas ela e nada mais.

### 🎯 A SUA MISSÃO (TESTE MENTAL DA ÚLTIMA TAREFA)
Realize uma conferência e revisão minuciosa:
1. Revise passo a passo tudo o que você fez no ciclo anterior;
2. Faça um pente-fino crítico simulando a execução real na sua "mente";
3. Feita a simulação, me responda com absoluta certeza funcional:
   "Esse código validado passo a passo VAI FUNCIONAR DE PRIMEIRA no ambiente final ou deixamos passar algo que vai quebrar tudo?"

Verifique a entrega imediatamente anterior buscando passo a passo:
1. Referências fantasmas: Chamamos algum método que esquecemos de criar nessa rodada?
2. Furos de Sintaxe: Ponto e vírgula ausente, aspas não fechadas, erros de digitação soltos?
3. Funcionalidade direta: O desenvolvedor copia agora, cola na IDE, ele levanta sem Crash?

### 🗣️ CONFIRMAÇÃO OBRIGATÓRIA INICIAL
Comece sua resposta estritamente com a seguinte frase:
> *"🔎 MODO REVISOR LIGADO! Estou olhando EXCLUSIVAMENTE para a nossa última entrega que acabou de ser concluída. Vou revisar o que fiz passo a passo e passar um pente-fino mental pra garantir que, ao subir para o GitHub agora, essa alteração vai rodar lista, sem quebrar de primeira."*

---

## FORMATO DA SUA RESPOSTA:

### 🚦 VAI FUNCIONAR DE PRIMEIRA?
- Informe explicitamente o Status atual APENAS sobre o último bloco processado:
   - [✅ SINAL VERDE: Sim, conferi nossa última ação passo a passo e vai rodar de primeira!]
   - [🟡 SINAL AMARELO: Na teoria sim, mas notei um alerta na estrutura dessa exata última alteração...]
   - [🔴 SINAL VERMELHO: FIZ ASNEIRA! Acabei de rever meu último código e deixei um furo. Corrigindo agora!]

### 🛠️ CORREÇÕES ENCONTRADAS
*(Preencha apenas se achou algum erro de quebra. Se não achou nada, avise "A última alteração passou 100% sem erros.")*
- O que falhou: ...
- Como fica corrigido agora: ...

---

### ⚠️⚠️⚠️ DECISÃO CRÍTICA - PARE AGORA MESMO ⚠️⚠️⚠️

**ANTES de executar os comandos Git abaixo, você TEM a seguinte escolha:**

#### 🤝 OPÇÕES DISPONÍVEIS:

| Opção | Ação |
|-------|------|
| **s** ou **Sim** | CONFIRMAR → Executar o commit e enviar ao GitHub |
| **n** ou **Não** | CANCELAR → NÃO enviar nada, voltar ao trabalho |

#### ⚠️⚠️⚠️ REGRA ABSOLUTA - OBRIGATÓRIO SEGUIR ESTE PROTOCOLO:

1. **PARE** e mostre esta mensagem ao usuário
2. **AGUARDE** explicitamente a resposta do usuário
3. **NUNCA** execute comandos Git automaticamente sem confirmação
4. **SE** o usuário digitar "n" ou "não", CANCELE tudo e saia
5. **SE** o usuário digitar "s" ou "sim", continue com o fluxo

**🚨 O OpenCode NÃO pode, em hipótese alguma, executar o Git automaticamente sem perguntar primeiro! 🚨**

---

### ✅ FLUXO GITHUB (EXECUTE APENAS APÓS CONFIRMAÇÃO "s")

**PASSO 0 — Verificação e Configuração do Editor (Notepad++)**

```bash
# ============================================================
# CONFIGURAÇÃO DO EDITOR - NOTEPAD++ (OBRIGATÓRIO)
# ============================================================

# Define Notepad++ como editor do Git (forçado)
git config --global core.editor "notepad++ -multiInst -notabbar -nosession"

# Confirma configuração
echo "✅ Editor configurado: Notepad++"

# Verifica configuração atual
git config --get core.editor
```

**PASSO 1 — Capture o último commit**

!`git log -1 --pretty=%B`

**PASSO 2 — Monte a nova mensagem**

Com base no commit capturado acima:
1. Leia a primeira linha. Formato: `NN) Título`. Extraia `NN`.
2. Some +1 mantendo zero à esquerda. Ex: `02` → `03`, `09` → `10`.
3. Escreva o novo `* CONCLUDED:` com 3 a 5 tópicos numerados, sucintos e técnicos, baseados EXCLUSIVAMENTE no que você acabou de implementar na resposta imediatamente anterior a este comando.
4. Copie `* WORKING:`, `* NEXT STEPS:` e `* FUTURE FEATURES` do commit capturado acima sem alterar uma vírgula.

**PASSO 3 — Salve a mensagem em arquivo temporário**

Escreva a mensagem completa montada no PASSO 2 no arquivo `/tmp/commit_msg.txt`, respeitando as quebras de linha reais da estrutura. Execute:
```bash
cat > /tmp/commit_msg.txt << 'EOF'
NN) Título do commit

* CONCLUDED:
  1 tópico
  2 tópico
  3 tópico

* WORKING:
  [copiado do commit anterior]

* NEXT STEPS:
  [copiado do commit anterior]

* FUTURE FEATURES
  [copiado do commit anterior]

-----------------------------------------------------------------------
EOF
```

**PASSO 4 — Execute a sequência Git**

⚠️ **IMPORTANTE**: O comando `git commit --amend -e` é OBRIGATÓRIO para forçar a abertura do editor!

Um comando por linha, sem `&&`:
```bash
git add .
git commit -F /tmp/commit_msg.txt
GIT_EDITOR="notepad++ -multiInst -notabbar -nosession" 
git commit --amend -e
git push origin main
```

> **O parâmetro `-e` é OBRIGATÓRIO e força a abertura do editor para editar a mensagem.**
> 
> **O `GIT_EDITOR="notepad++ -multiInst -notabbar -nosession"` é o fallback de segurança que garante o Notepad++ abrir.**
> 
> O Notepad++ abrirá com a mensagem montada. Revise, ajuste o que quiser, salve e feche.
> O `git push origin main` finaliza o envio.