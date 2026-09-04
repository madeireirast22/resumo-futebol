# resumo-futebol

Resumo agregado de estatísticas de futebol, com foco em **1º tempo**.
Espelho de leitura, atualizado por um robô uma vez por dia.

Só números agregados e derivados: média por competição, linha de cada time
separada por mando, e a sequência crua dos últimos 10 jogos. A base
jogo a jogo não está aqui.

## Por onde começar

- [`indice.json`](https://raw.githubusercontent.com/madeireirast22/resumo-futebol/main/indice.json) — a régua das competições, sem os times (~17 KB).
  Leia primeiro e escolha a competição.
- `<liga>-<ano>.json` — a competição inteira (~148 a 280 KB).

## Formato

```
metricas["escanteios_ht"] = { jogos, media, overs: { "1.5": 93.2, ... } }
  media = total do jogo (os dois times somados) no período
  overs = em quantos % dos jogos o total PASSOU da linha (frequência real)

times["Flamengo"]["escanteios_ht"] = {
  jogos, proMedia, contraMedia, totalMedia,
  casa: { jogos, pro, contra }, fora: { jogos, pro, contra } }

times["Flamengo"].ultimos = [ { data, adv, mando, golsHt, golsFt,
  ht: { esc, ch, car }, ft: { esc, ch, car } }, ... ]
  Todo par é [feitos, cedidos] do ponto de vista daquele time.
  mando: "C" = em casa, "F" = fora. Mais recente por último.
```

Sufixos: `_ht` = 1º tempo, `_ft` = jogo inteiro.

## Cuidados

- Brasileirão A e B sustentam projeção de confronto (24 a 38 jogos por time).
- Copas não: por time são 2 a 6 jogos. Ali use só a régua da competição.
- Série B 2025 está incompleta, e o que falta não é aleatório: é o returno
  sem o turno.
- Não há jogos futuros nem odds.

Dado derivado da SofaScore. Aqui só entram agregados, não a base dela.
