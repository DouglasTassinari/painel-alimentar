# Painel Alimentar

Registro de calorias com gasto energético calculado a partir dos seus próprios dados, em um único arquivo HTML. Sem instalação, sem servidor, sem conta, sem rastreamento.

**[Abrir o painel →](https://douglastassinari.github.io/painel-alimentar/)**

---

## Por que existe

Apps de contagem e planilhas falham por dois motivos: o registro custa caro e a meta é chutada. A meta padrão desses apps sai de uma fórmula genérica que não sabe nada sobre você — e quando o peso não reage, não há como distinguir "comi mais do que anotei" de "meu gasto é menor que a fórmula diz".

Este painel resolve o segundo problema. **Nada nele é pré-configurado.** O metabolismo é recalculado a cada pesagem, e depois de duas semanas de registro ele abandona a fórmula e passa a usar o gasto medido dos seus dados.

## Como funciona

**Registro** — você adiciona quantas refeições fizer no dia, com o número de calorias. Sem refeição fixa e sem horário obrigatório: quem faz 2 refeições lança 2. O nome é opcional.

Para estimar as calorias, um botão copia um prompt pronto — você cola junto com a foto do prato no GPT (ou Claude) e traz o número de volta.

**O gasto é calculado, não escolhido:**

1. Enquanto não há dados suficientes, usa Mifflin-St Jeor sobre a **sua última pesagem**. Cada peso novo reescreve o metabolismo basal e a meta do dia.
2. Depois de 14 dias registrados e duas pesagens afastadas, troca a fórmula pelo **seu gasto real**:

```
gasto = consumo médio + (peso perdido × 7700) / dias
```

Só existe um gasto que explica ao mesmo tempo o que você comeu e o que a balança mostrou. Esse número vale mais que qualquer fórmula, e se refaz a cada pesagem nova.

**Isso corrige o erro da estimativa por foto.** Estimar caloria por imagem erra 20–30%. Mas se o erro é consistente, ele se cancela: um consumo registrado 20% abaixo do real produz um gasto calculado 20% menor, e o déficit efetivamente entregue continua o mesmo. Por isso a regra é usar sempre a mesma ferramenta e o mesmo prompt — consistência importa mais que exatidão.

O cálculo só é aceito com pelo menos 10 dias registrados, 60% de cobertura do período e duas pesagens a 14+ dias de distância. Faltando qualquer uma dessas condições, o painel volta pra fórmula em vez de inventar número.

**A pergunta que ele responde: seus hábitos atuais te levam a emagrecer ou engordar?**

```
saldo do dia    = calorias consumidas − gasto energético
saldo médio     = soma dos saldos ÷ dias registrados   (últimos 30 dias)
kg por semana   = saldo médio × 7 ÷ 7700
```

A janela é de 30 dias, ou todo o histórico se houver menos que isso. **Dias sem registro não entram como zero** — ficam de fora da média, e a divisão é só pelos dias registrados. A projeção parte da última pesagem e aplica essa variação dia a dia; o gráfico mostra a linha do peso real pelas pesagens e, tracejada, para onde o saldo calórico atual leva nas 4 semanas seguintes.

O painel exibe junto quantos dias sustentam a conta, porque 25 dias registrados e 4 dias registrados produzem o mesmo formato de frase e valem coisas muito diferentes.

**Painel** — média de consumo de 7 dias, déficit médio contra o gasto, tendência de peso pelo saldo calórico, sequência de dias, peso em média móvel de 7 dias, tendência por regressão dos últimos 90 dias, barras de progresso com previsão de data, gráfico de consumo diário colorido pela meta e mapa de calor de 12 semanas.

**Lembretes** — um site não notifica com o app fechado (no iPhone é bloqueado, no Android é instável). Em vez de prometer o que não funciona, o painel gera um arquivo `.ics` com lembretes recorrentes nos horários que você escolher: importa uma vez no Google Agenda ou no Calendário e o próprio celular te lembra.

## Dados

Ficam em `localStorage`, só no seu aparelho. Nada sai do navegador — não há backend, analytics ou requisição de rede.

A contrapartida é que **limpar os dados do navegador apaga tudo**. Use `Dados → Exportar backup` de vez em quando. O JSON exportado volta pelo botão de importar, inclusive em outro aparelho.

## Uso no celular

Abra o link, e em seguida:

- **Android/Chrome** — menu ⋮ → *Adicionar à tela inicial*
- **iOS/Safari** — botão compartilhar → *Adicionar à Tela de Início*

Vira ícone e abre em tela cheia, como app nativo.

## Rodar localmente

```bash
git clone https://github.com/DouglasTassinari/painel-alimentar.git
```

Abra o `index.html` no navegador. Sem build, sem dependências. Os gráficos são SVG gerado à mão e funciona offline.

## Importar histórico de balança

Balanças com app (Mi Fit/Zepp, Renpho e similares) costumam exportar `.xlsx`. Converta para o formato de backup:

```json
{
  "cfg": {
    "altura": 175, "idade": 30, "sexo": "m",
    "atividade": 1.2, "deficit": 500,
    "meta1": 91.6, "meta2": 76.3, "partida": 82.0
  },
  "dias": {},
  "medidas": [
    { "data": "2026-01-15", "peso": 80.5, "gordura": 22.0, "visceral": 10.0 }
  ]
}
```

Em `medidas`, só `data` e `peso` são obrigatórios. Em `dias`, o formato é `{"2026-01-15": {"refs": [{"n": "Almoço", "k": 850, "h": "12:30"}]}}`. Importe em `Dados → Importar backup`.

`atividade` é o multiplicador sobre o metabolismo basal: 1.2 sedentário, 1.375 leve, 1.55 moderado, 1.725 intenso. Ele só vale até o painel ter dados suficientes para calcular o gasto real — a partir daí é ignorado.

## O que ele faz e o que ele não faz

**Não faz:** dizer o que comer, montar cardápio, ou emagrecer alguém. Quem causa perda de peso é o déficit calórico, e o painel não cria déficit nenhum.

**Faz:** medir seu consumo, descobrir seu gasto real, e mostrar a distância entre os dois. Se o peso não reage com o consumo registrado abaixo do gasto, isso aponta pra subestimativa nas fotos ou dias não registrados — e essa também é uma resposta útil, que uma planilha não dá.

## Aviso

Não é dispositivo médico e não substitui acompanhamento profissional. É uma ferramenta de registro pessoal. Decisões sobre dieta, restrição calórica ou exercício merecem orientação de nutricionista ou médico.

## Licença

MIT
