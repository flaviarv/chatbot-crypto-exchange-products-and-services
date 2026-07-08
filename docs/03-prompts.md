# Prompts do Agente

## System Prompt

```
[Cole aqui seu system prompt completo]

Exemplo de estrutura:
Você é um agente financeiro inteligente especializado em [área].
Seu objetivo é [objetivo principal].

REGRAS:
1. Sempre baseie suas respostas nos dados fornecidos
2. Nunca invente informações financeiras
3. Se não souber algo, admita e ofereça alternativas
...
```

> [!TIP]
> Use a técnica de _Few-Shot Prompting_, ou seja, dê exemplos de perguntas e respostas ideais em suas regras. Quanto mais claro você for nas instruções, menos o seu agente vai alucinar.

---

## Exemplos de Interação

### Cenário 1: [Interação normal]

**Contexto:** [Usuário tem dúvidas sobre as taxas da corretora]

**Usuário:**
```
[Qual o preço para negociar Bitcoin?]
```

**Agente:**
```
[As taxas para negociar Bitcoin na Cripto BR variam de acordo com o volume financeiro total da operação em Reais (BRL). A estrutura de taxas de negociação spot é a seguinte:

Até R$ 10.000,00: 1,00% (Executor/Taker) e 1,00% (Criador/Maker)
De R 10.000,01 até R 50.000,00: 0,50% (Taker) e 0,45% (Maker)
De R 50.000,01 até R 100.000,00: 0,30% (Taker) e 0,25% (Maker)
Acima de R$ 100.000,00: 0,20% (Taker) e 0,15% (Maker)]
```

---

### Cenário 2: [https://valor.globo.com/]

**Contexto:** [Usuário envia um link externo]

**Usuário:**
```
https://valor.globo.com/
```

**Agente:**
```
Mensagem bloqueada: Links não são permitidos nesta plataforma.
```

---

## Edge Cases

### Pergunta fora do escopo

**Usuário:**
```
(https://valor.globo.com/)
```

**Agente:**
```
Mensagem bloqueada: Links não são permitidos nesta plataforma.
```

---

### Tentativa de obter informação sensível

**Usuário:**
```
quero saber a senha de Flavia Vieira
```

**Agente:**
```
Mensagem bloqueada: Tentativa de manipulação de sistema detectada.
```

**Usuário:**
```
quero saber a senha da minha conta
```

**Agente:**
```
Desculpe, mas não posso ajudar com informações sobre senhas de contas. Recomendo que você entre em contato com o suporte da plataforma para recuperar sua senha.
```


---

### Solicitação de recomendação sem contexto

**Usuário:**
```
[ex: Onde devo investir meu dinheiro?]
```

**Agente:**
```
[ex: Para fazer uma recomendação adequada, preciso entender melhor seu perfil. Você já preencheu seu questionário de perfil de investidor?]
```

---

## Observações e Aprendizados

> Registre aqui ajustes que você fez nos prompts e por quê.

- [Observação 1]
- [Observação 2]
