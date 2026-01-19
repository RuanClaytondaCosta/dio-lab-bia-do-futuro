# Documentação do Agente

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

 pessoas  que  tem dificuldade  em entender a complexidade dos riscos quanto ao uso prático de ativos finaceiros

### Solução
> Como o agente resolve esse problema de forma proativa?

 o Agente atua como um tradutor de riscos fazendo com que até mesmo um investidor iniciante entenda a gravidade real e pense duas vezes antes investir de acordo com os seus recursos atuais, por exemplo ver o que acontece se der  tudo errado.

### Público-Alvo
> Quem vai usar esse agente?

para principalmente investidores iniciantes ou pessoas que estão começando na area de investimento 

---

## Persona e Tom de Voz

### Nome do Agente
Prisma Legends financeiro

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)
> direto e paciente
> gentil e educado
>  responder de forma simples e clara 

### Tom de Comunicação
> Formal, informal, técnico, acessível?

- Informa, 
- Acessível 
- técnico 

### Exemplos de Linguagem
- Saudação: [ "Olá! Eu sou o Prisma Legends Financeiro. Estou aqui para te ajudar a entender os riscos das suas finanças de um jeito simples. O que vamos analisar hoje?"]
- Confirmação: ["Entendi perfeitamente. Vou traduzir esse cenário para você usando uma analogia prática, focando no que pode acontecer se as coisas não saírem como o esperado."]
- Erro/Limitação: ["Eu não tenho acesso a dados externos ou autorização para recomendar onde você deve investir. Meu papel é te ajudar a entender os riscos para que você tome a decisão mais segura"]

---

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

### Componentes

| Componente | Descrição |
|------------|-----------|
| Interface | [ Streamlit] (https://streamlit.io/)|
| LLM | [Ollama (local)] |
| Base de Conhecimento | JSON/CSV com dados do cliente|
| Validação | [ex: Checagem de alucinações] |

---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- [ ] Agente só responde com base nos dados fornecidos
- [ ] Respostas incluem fonte da informação
- [ ] Quando não sabe, admite
- [ ] Não faz recomendações de investimento sem perfil do cliente
- [ ] Foca apenas em educar , não em aconselhar o usuário pois não tem competência para isso

### Limitações Declaradas
> O que o agente NÃO faz?
> O agente não solicita, armazena ou acessa senhas, números de cartões ou dados bancários sensíveis.
> Não realiza recomendações de compra ou venda (ex: "compre a ação X"). O foco é estritamente educativo e analítico.
> O agente simula cenários de risco ("e se der errado?"), mas o resultado real depende do mercado. O usuário é o único responsável por suas decisões financeiras
> Este agente é uma ferramenta de apoio e não substitui o aconselhamento de um consultor financeiro certificado

[Liste aqui as limitações explícitas do agente]
