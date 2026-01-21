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
>
> para simplificar, podemos  "injetar" os dados em nossop prompt, garantindo que o agente Prisma Legends Finaceiro possa entender melhor o contexto do que de fato o cliente deseja, mas claro que o correto é que devemos ter em soluções mais robustas ter flexibilidade para melhor eficiência.

```
DADOS  DO CLIENTE E PERFIL:{data/perfil_investidor.json}
{
  "nome": "João Silva",
  "idade": 32,
  "profissao": "Analista de Sistemas",
  "renda_mensal": 5000.00,
  "perfil_investidor": "moderado",
  "objetivo_principal": "Construir reserva de emergência",
  "patrimonio_total": 15000.00,
  "reserva_emergencia_atual": 10000.00,
  "aceita_risco": false,
  "metas": [
    {
      "meta": "Completar reserva de emergência",
      "valor_necessario": 15000.00,
      "prazo": "2026-06"
    },
    {
      "meta": "Entrada do apartamento",
      "valor_necessario": 50000.00,
      "prazo": "2027-12"
    }
  ]
}

TRANSAÇOES DO CLIENTE {data/transacoes.csv}

data,descricao,categoria,valor,tipo
2025-10-01,Salário,receita,5000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,450.00,saida
2025-10-05,Netflix,lazer,55.90,saida
2025-10-07,Farmácia,saude,89.00,saida
2025-10-10,Restaurante,alimentacao,120.00,saida
2025-10-12,Uber,transporte,45.00,saida
2025-10-15,Conta de Luz,moradia,180.00,saida
2025-10-20,Academia,saude,99.00,saida
2025-10-25,Combustível,transporte,250.00,saida



HISTORICO DE ATENTIMENTO DO CLIENTE:{data/historico_atendimento.csv}

data,canal,tema,resumo,resolvido
2025-09-15,chat,CDB,Cliente perguntou sobre rentabilidade e prazos,sim
2025-09-22,telefone,Problema no app,Erro ao visualizar extrato foi corrigido,sim
2025-10-01,chat,Tesouro Selic,Cliente pediu explicação sobre o funcionamento do Tesouro Direto,sim
2025-10-12,chat,Metas financeiras,Cliente acompanhou o progresso da reserva de emergência,sim
2025-10-25,email,Atualização cadastral,Cliente atualizou e-mail e telefone,sim

PRODUTO DISPONIVEL PARA ENSINO:{data/produtos_financeiros.json}
[
  {
    "nome": "Tesouro Selic",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "100% da Selic",
    "aporte_minimo": 30.00,
    "indicado_para": "Reserva de emergência e iniciantes"
  },
  {
    "nome": "CDB Liquidez Diária",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "102% do CDI",
    "aporte_minimo": 100.00,
    "indicado_para": "Quem busca segurança com rendimento diário"
  },
  {
    "nome": "LCI/LCA",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "95% do CDI",
    "aporte_minimo": 1000.00,
    "indicado_para": "Quem pode esperar 90 dias (isento de IR)"
  },
  {
    "nome": "Fundo Multimercado",
    "categoria": "fundo",
    "risco": "medio",
    "rentabilidade": "CDI + 2%",
    "aporte_minimo": 500.00,
    "indicado_para": "Perfil moderado que busca diversificação"
  },
  {
    "nome": "Fundo de Ações",
    "categoria": "fundo",
    "risco": "alto",
    "rentabilidade": "Variável",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil arrojado com foco no longo prazo",
    "alerta_vulnerabilidade" : "qaundo tiver juros Altos é recomendavel não investir"
  }
]

```


---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.
>
> Exemplo baseados nos dados que ja estão constados no arquivo CSV

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

RESUMO FINANCEIRO (OUTUBRO/2025):
- Entradas: R$ 5.000,00
- Saídas Totais: R$ 2.488,90
- Saldo Líquido do Mês: R$ 2.511,10
- Maiores Gastos: Aluguel (R$ 1.200,00) e Supermercado (R$ 450,00).

PRODUTOS DISPONÌVEIS PARA EXPLICAR:
- Renda Fixa: Tesouro Selic, CDB Liquidez Diária, LCI/LCA.
- Renda Variável/Fundos: Fundo Multimercado, Fundo de Ações.
- REGRA CRÍTICA: Se sugerir Fundo de Ações, verifique o "Alerta de Vulnerabilidade" (não investir se houver juros altos ou dívidas).
...
```
