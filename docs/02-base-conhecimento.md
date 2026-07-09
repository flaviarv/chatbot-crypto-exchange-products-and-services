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

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
