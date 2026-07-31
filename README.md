# Painel Alimentar

Registro alimentar por semáforo, em um único arquivo HTML. Sem instalação, sem servidor, sem conta, sem rastreamento.

**[Abrir o painel →](https://douglastassinari.github.io/painel-alimentar/)**

---

## Por que existe

Apps de contagem de calorias e planilhas de controle falham pelo mesmo motivo: **o custo de registro é alto e o retorno é lento**. Estimar a caloria de cinco refeições por dia é trabalho de pesquisa e digitação que quase ninguém sustenta por mais de duas semanas.

Este painel parte de uma premissa diferente: **o registro diário tem que custar menos de 10 segundos**, ou não vai acontecer. O que ele mede não é caloria — é aderência.

Um plano de 70% seguido bate um plano de 100% abandonado.

## Como funciona

**Registro** — quatro refeições, três botões cada:

| | |
|---|---|
| 🟢 | no plano |
| 🟡 | escorreguei |
| 🔴 | fora |

Dia inteiro registrado em quatro toques. Zero digitação, zero consulta a tabela nutricional. Dá pra voltar e preencher dias esquecidos.

**Painel**

- Aderência de 7 e 30 dias, ponderada (🟢 = 1, 🟡 = 0,5, 🔴 = 0)
- Sequência de dias registrados
- Peso em **média móvel de 7 dias** — a balança oscila cerca de 1 kg só por variação de água e sódio; a média móvel é o que revela tendência real
- Tendência em kg/semana por regressão linear sobre os **últimos 90 dias** (a série inteira dilui o momento atual em platôs antigos)
- Barras de progresso até duas metas, com previsão de data derivada da tendência
- Mapa de calor de 12 semanas — é onde padrões semanais aparecem

**Detecção de bioimpedância derivada** — muitas balanças domésticas não medem composição corporal de forma independente: calculam por fórmula a partir de peso, altura, idade e sexo. O painel testa isso nos seus próprios dados, correlacionando % de gordura com peso e checando se pesos repetidos devolveram valores idênticos. Se a correlação passar de 0,97, ele avisa que aquele número não é informação nova.

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

Abra o `index.html` no navegador. É só isso — sem build, sem dependências. Os gráficos são SVG gerado à mão, funciona offline.

## Importar histórico de balança

Balanças com app (Mi Fit/Zepp, Renpho e similares) costumam exportar `.xlsx`. Converta para o formato de backup:

```json
{
  "cfg": { "altura": 175, "meta1": 91.6, "meta2": 76.3, "partida": 82.0 },
  "dias": {},
  "medidas": [
    { "data": "2026-01-15", "peso": 80.5, "gordura": 22.0, "visceral": 10.0 }
  ]
}
```

`gordura`, `visceral`, `agua` e `musculo` são opcionais — só `data` e `peso` são obrigatórios. Importe em `Dados → Importar backup`.

## Aviso

Não é dispositivo médico e não substitui acompanhamento profissional. É uma ferramenta de registro pessoal. Decisões sobre dieta, restrição calórica ou exercício merecem orientação de nutricionista ou médico.

## Licença

MIT
