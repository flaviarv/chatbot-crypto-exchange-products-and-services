# Prompts do Agente

## System Prompt

```
prompt_consulta = ChatPromptTemplate.from_messages([
    ("system", "Responda exclusivamente com o conteúdo fornecido no contexto."),
    ("human", "{query}\n\nContexto:\n{contexto}\n\nResposta:")
])

cadeia = prompt_consulta | modelo | StrOutputParser()

def responder(pergunta: str):
    trechos = retriever.invoke(pergunta)
    contexto = "\n\n".join(t.page_content for t in trechos)
    return cadeia.invoke({"query": pergunta, "contexto": contexto})
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
eu estou triste
```

**Agente:**
```
Sinto muito que você esteja se sentindo assim. Se precisar conversar ou se houver algo específico em que eu possa ajudar, estou aqui para isso.

```

## Observações e Aprendizados

Após diversos testes de QA, ficou evidente que instruir um agente, por meio de um ChatPromptTemplate, a responder apenas com base na base de conhecimento não é uma defesa eficaz contra jailbreaks semânticos. Como essa restrição existe apenas no nível do prompt, ela pode ser contornada por instruções maliciosas ou ambiguidades introduzidas pelo usuário.

Uma abordagem mais robusta é implementar Guardrails. Diferentemente das instruções do prompt, os Guardrails atuam como uma camada independente de controle, validando entradas e saídas do modelo e aplicando regras de segurança antes que a resposta seja entregue ao usuário. Isso torna o agente significativamente mais resistente a tentativas de manipulação e reduz a probabilidade de respostas fora do escopo da base de conhecimento.

É um problema que precisa ser corrijido. 
