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
>
> Existem duas maneirasde  intejar os dados diretamente no prompt, ou CTRL + C, CTRIL + V) ou carregar os código abaixo:

```python

import pandas as pd
import json

def carregar_dados():
    # Carregamento conforme os caminhos da imagem de documentação
    historico = pd.read_csv('data/historico_atendimento.csv')
    transacoes = pd.read_csv('data/transacoes.csv')
    
    with open('data/perfil_investidor.json', 'r', encoding='utf-8') as f:
        perfil = json.load(f)
        
    with open('data/produtos_financeiros.json', 'r', encoding='utf-8') as f:
        produtos = json.load(f)
        
    return historico, transacoes, perfil, produtos

def prisma_legends_analise(transacoes, produtos):
    # Lógica para resolver a complexidade dos riscos
    receita = transacoes[transacoes['tipo'] == 'entrada']['valor'].sum()
    despesas = transacoes[transacoes['tipo'] == 'saida']['valor'].sum()
    saldo_disponivel = receita - despesas
    
    print(f"--- Relatório Prisma Legends ---")
    print(f"Saldo atual detectado: R$ {saldo_disponivel:.2f}")

    # Exemplo de recomendação baseada no arquivo JSON fornecido
    for p in produtos:
        if saldo_disponivel >= p['aporte_minimo']:
            print(f"Sugestão: {p['nome']} | Risco: {p['risco']}")

# Execução
if __name__ == "__main__":
    h, t, perf, prod = carregar_dados()
    prisma_legends_analise(t, prod)
```


### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

```
DADOS 


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
