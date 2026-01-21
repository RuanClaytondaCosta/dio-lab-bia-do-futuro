# Base de Conhecimento

## Dados Utilizados


| Arquivo | Formato | Para que serve no Prisma Legends Financeiro |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores, se |
| `perfil_investidor.json` | JSON | Personalizar as explicações sobre as necessidades do cliente e adaptar se a Ele para a idenficação de riscos |
| `produtos_financeiros.json` | JSON | conhecer o que o que o cliente esta disponível no momento para adaptar  se  as suas necessidades |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente para assim analisar as consequencias do resultado da ação  |

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

 foi adicionado  no Fundo de Ações  a analise de segurança, verificando se o cliente possui juros altos

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

[ex: Os JSON/CSV são carregados no início da sessão e incluídos no contexto do prompt]

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

[Sua descrição aqui]

---

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
