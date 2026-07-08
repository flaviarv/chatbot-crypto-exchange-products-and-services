# Documentação do Agente

## Caso de Uso

### Problema

Manter uma equipe numerosa de atendimento representa um custo elevado para qualquer corretora de criptomoedas. Além de salários e encargos, há despesas com treinamento, supervisão, escalas de trabalho e a necessidade de expandir a equipe conforme a base de clientes cresce.

Um chatbot com IA reduz significativamente esses custos ao responder dúvidas frequentes de forma instantânea, consistente e disponível 24 horas por dia. Isso aumenta a eficiência de capital da empresa, permitindo que recursos financeiros sejam direcionados para áreas que geram mais valor, como segurança, desenvolvimento de produtos, compliance e inovação. Com isso, a corretora consegue atender mais clientes com menor custo operacional, mantendo qualidade e escalabilidade no atendimento.

### Solução

O chatbot de IA foi desenvolvido para oferecer respostas rápidas e confiáveis às principais dúvidas dos clientes sobre os produtos e serviços da corretora de criptomoedas. Em vez de depender exclusivamente de atendentes humanos para questões repetitivas, o sistema automatiza grande parte do atendimento, reduzindo o tempo de espera e melhorando a experiência do usuário.

Como solução, o chatbot aumenta a eficiência operacional, reduz custos com atendimento de primeiro nível, melhora a escalabilidade do suporte e proporciona respostas imediatas aos clientes. O resultado é um atendimento mais ágil, maior produtividade da equipe e uma utilização mais eficiente dos recursos da corretora.

### Público-Alvo

Como fica na página principal do site, todos os tipos de clientes pode usar o Agente de IA para tirar dúvidas sobre produtos e serviços da exchange. Clientes premium tendem a usar menos, pois possuem atendimento personalizado. 

----------------------------------------------------------------------------------------------------------

## Persona e Tom de Voz

### Nome do Agente
Não possui nome. 

### Personalidade
A personalidade do agente é consultivo e direto. 

### Tom de Comunicação
Formal, técnico e seco. Está mais próximo de um robô do que humano. 

--------------------------------------------------------------------------------------------------------------

## Arquitetura

### Diagrama

```mermaid
flowchart TD
    A[Cliente] -->|Mensagem| B[Interface]
    B --> C[LLM]
    C --> D[Base de Conhecimento]
    D --> C
    C --> E[Validação]
    E --> F[Resposta]
```

```mermaid
flowchart TB
    n1["app.py"] --> n2["Core/"]
    n2 --> n3["__init__.py"]
    n3 --> n4["main.py"]
    n4 <--> n5["guardrails.py"]
    n4 --> n8["chroma db/"]
    n4 --> n6["Documentos/"]
    n4 --> n1
    n6 --> n7["base_de_conhecimento.PDF"]
    n7 --> n4
    n8 --> n9["Embeddings"]
    n9 --> n4

    %% estilos
    style n1 fill:#C8E6C9,stroke:#000000
    style n2 fill:#BBDEFB
    style n3 fill:#C8E6C9
    style n4 fill:#C8E6C9
    style n5 fill:#C8E6C9
    style n8 fill:#BBDEFB
    style n6 fill:#BBDEFB
    style n7 fill:#FFF9C4
    style n9 fill:#ffffff
```



### Componentes

| Componente | Descrição |
|------------|-----------|

| Interface | Chatbot da Steamlite |

| LLM | gpt-4o-mini |

| Base de Conhecimento | Arquivo PDF com informações sobre produtos e serviços da corretora, armazenados de forma local |

| Banco de Dados Vetorial | Chroma DB (open source) |

| Guardrails | Guardrails da OpenAI |


### Banco de Dados Vetorial

O projeto usou um  Banco de Dados Vetorial open source chamado Chroma DB. 

Um banco de dados vetorial armazena dados como representações matemáticas chamadas vetores.

Modelos de linguagem não entendem textos da mesma forma que os seres humanos. Antes de armazenar ou pesquisar um documento, eles transformam palavras, frases e parágrafos em representações matemáticas chamadas embeddings. Esses embeddings são vetores numéricos que capturam o significado semântico do conteúdo, fazendo com que textos com ideias semelhantes fiquem próximos uns dos outros em um espaço vetorial, mesmo quando utilizam palavras diferentes.

Quando um usuário faz uma pergunta, essa pergunta também é convertida em um embedding. Em seguida, o sistema procura quais documentos possuem vetores mais próximos, recuperando os trechos mais relevantes para fornecer contexto ao modelo de linguagem antes da geração da resposta.

É nesse momento que o banco de dados vetorial se torna indispensável. Em vez de enviar toda a documentação da corretora para o modelo de IA a cada consulta, o banco vetorial realiza uma busca por similaridade entre embeddings e retorna apenas os documentos realmente relacionados à pergunta do usuário. Dessa forma, o modelo recebe somente as informações necessárias para responder.

Essa arquitetura reduz significativamente a quantidade de tokens enviados ao modelo de linguagem, diminuindo o custo de inferência e melhorando o tempo de resposta. Sem um banco de dados vetorial, seria necessário enviar uma quantidade muito maior de documentos para que a IA encontrasse a resposta correta, aumentando o consumo de tokens, a latência e os custos operacionais. À medida que a base de conhecimento cresce, essa abordagem se torna cada vez menos eficiente e mais cara.

Por isso, bancos de dados vetoriais são um dos pilares da arquitetura de agentes de IA modernos. Eles permitem que o sistema localize rapidamente as informações mais relevantes, mantenha a qualidade das respostas e escale para grandes volumes de documentação sem elevar proporcionalmente os custos de processamento.

### Guardrails

Guardrails são mecanismos de segurança e governança que controlam o comportamento de um chatbot de IA, definindo limites sobre o que ele pode responder e como deve responder. Em vez de permitir que o modelo gere qualquer resposta, os guardrails aplicam regras para garantir que as informações fornecidas sejam consistentes com as políticas da empresa, requisitos regulatórios e boas práticas de segurança.

No mercado financeiro, sua importância é ainda maior. Instituições financeiras lidam diariamente com informações sensíveis, produtos regulados e decisões que podem impactar o patrimônio dos clientes. Um chatbot sem controles pode gerar respostas incorretas, divulgar informações confidenciais ou fornecer orientações inadequadas sobre investimentos e operações financeiras.

Ao implementar guardrails, é possível bloquear solicitações fora do escopo do atendimento, impedir o vazamento de dados, detectar conteúdo inadequado, validar respostas antes de enviá-las ao usuário e direcionar casos complexos para atendentes humanos. Dessa forma, a instituição reduz riscos operacionais, fortalece a conformidade regulatória e aumenta a confiabilidade das respostas geradas pela IA.

Em um chatbot para corretoras de criptomoedas, os guardrails são um componente essencial para garantir que a inteligência artificial atue de forma segura, previsível e alinhada às políticas internas e às exigências de compliance, oferecendo uma melhor experiência ao cliente sem comprometer a segurança da operação.

### Base de conhecimento

-------------------------------------------------------------------------------------------------------------------------

## Segurança e Anti-Alucinação

### Estratégias Adotadas para Base de Consulta

> Agente só responde perguntas se o conteúdo estiver dentro da base de consulta (documentos/manual_cripto_br_rag.pdf) 

> Se a pergunta não estiver na base de consulta, o agente responde que a pergunta está fora de contexto. O código aplica restrição limitando a fonte de conhecimento da IA é ("system", "Responda exclusivamente com o conteúdo fornecido no contexto.")

> Os embeddings da base de consulta são armazenados localmente através do Chroma DB

### Estratégias Adotadas para Guardrails (Bloqueio de links externos)

> Bloqueio de links externos: Possui um bloco de código que detecta URLs com http, https e www
  
- Bloqueio de links externos: bloco de código com lógica Booleana: se detectar uso de link externo (TRUE), retorna com     "Links não são permitidos nesta plataforma."


## Limitações do Agente de IA (teste de QA)

> O Gardrails da OpenAI ainda não está configurado para prevenir jailbreaks semânticos (apenas links externos). Nos testes de QA, deu conselhos para desabafar como se fosse um psicólogo.

> O método ("system", "Responda exclusivamente com o conteúdo fornecido no contexto.") para prevenir jailbreakings semânticos não é o melhor. Idealmente, isso deve estar melhor estruturado em guardrails.py e não em main.py. Talvez seja uma boa ideia trocar o Guardrails da OpenAI pelo Model Armor do Google (na minha experiência, funcionou muito melhor).

> Ainda não está suficientemente inteligente a ponto de entender que a frase "Qual a taxa do Bitcoin?" é a mesma coisa que "Qual a taxa para negociar Bitcoin?"

> Guardrails bloqueia corretamente links externos com http, https e www, mas não bloqueia links com .com, .io, .org, dentre outros.

> O chatbot demora para carregar no Streamlite por causa do Banco de Dados Vetorial. Precisa acelerar a inicialização do programa.
