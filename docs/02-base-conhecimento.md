# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `base_de_dados_rag.PDF` | PDF | Informações sobre produtos e serviços da corretora Cripto BR, como taxas, ativos negociados e outros. |

---

## Estratégia de Integração

### Como os dados são carregados?

Antes de qualquer consulta, o Chroma DB realiza o processo de chunking da base de conhecimento, dividindo o conteúdo em pequenos trechos e gerando os respectivos embeddings. Esses vetores são armazenados no diretório chroma_db/.
A partir desse momento, o LLM não consulta mais diretamente o arquivo PDF utilizado como base de conhecimento. Em vez disso, ele recupera apenas os embeddings mais relevantes armazenados no Chroma DB, tornando a busca por informações mais rápida e eficiente.

## Estrutura da Base de Conhecimento em PDF

Estrutura do documento: 

**Criptomoedas Disponíveis para Negociação**

- Ativos Suportados: Lista detalhada de 20 ativos digitais selecionados para negociação spot (à vista).  

- Redes Oficiais: Especificação exata das redes suportadas para depósito e saque de cada criptoativo (ex: Native SegWit para BTC, ERC-20 para ETH, etc.).  

- Requisitos Especiais: Identificação de ativos que exigem parâmetros adicionais nas transações, como TAG para Ripple (XRP) ou Memo para Cosmos (ATOM) e Stellar (XLM).  

**Estrutura de Taxas e Operações (Moeda Fiduciária e Spot)**

- Negociação Spot: Tabela regressiva de taxas para Criador (Maker) e Executor (Taker), calculada estritamente com base no volume financeiro individual de cada operação em Reais (BRL). As faixas variam de "Até R$ 10.000,00" (taxa de 1,00%) a "Acima de R$ 100.000,00" (0,20% Taker / 0,15% Maker).  

- Depósitos via PIX: Gratuitos, com valor mínimo de R$ 10,00.  

- Saques via PIX: Taxa fixa de R$ 2,90 por operação, com valor mínimo de R$ 20,00.  

- Prazos: Funcionamento 24/7 com tempo médio de processamento de até 10 minutos para transações via PIX.  

**Programa de Afiliados**

- Mecanismo: Permite a monetização através de links de indicação, gerando comissões recorrentes e vitalícias baseadas nas taxas de negociação dos indicados.  

- Níveis de Comissionamento: Dividido em 3 níveis de acordo com o número de indicados ativos: Bronze (20%), Prata (30%) e Ouro (40%).  

- Definição de Usuário Ativo: O indicado precisa realizar ao menos uma operação de qualquer valor nos últimos 90 dias.  

- Pagamento: Apuração diária e pagamento mensal em BRL ou USDT, à escolha do afiliado.  

**Mesa de Operações OTC (Over-the-Counter)**

- Público-Alvo: Destinada a investidores de grande porte (institucionais ou PF) para movimentar altos volumes sem impactar o livro de ordens público (evitando slippage).  

- Regras: Volume mínimo de R$ 100.000,00 por operação, cotação personalizada e fixada direto com o operador, e liquidação imediata (D+0).  

- Custos: Taxa nominal zero, aplicando um spread comercial fixo embutido na cotação que varia de acordo com o volume (0,40% a 0,10%).  

**Atendimento VIP (Alta Renda)**

- Critérios de Elegibilidade: Enquadramento automático para quem mantiver patrimônio custodiado ≥ R$ 500.000,00 ou atingir volume mensal de negociação > R$ 1.000.000,00.  

- Benefícios: Gerente de conta dedicado (WhatsApp/telefone/e-mail), suporte prioritário 24/7 (resposta em até 2 minutos), relatórios semanais de Research, acesso antecipado a pré-lançamentos, isenção na taxa de saque PIX e desconto permanente de 20% nas taxas spot padrão.  

- Diretrizes para a IA: Instrução explícita de como o agente deve proceder ao identificar um cliente com perfil VIP, orientando-o a usar o comando /falar-com-gerente ou abrir chamado na categoria correspondente após confirmar os requisitos.  
