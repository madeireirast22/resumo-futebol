# resumo-futebol

Resumo agregado de estatísticas de futebol, com foco em **1º tempo**.
Espelho de leitura, atualizado por um robô uma vez por dia.

Só números agregados e derivados: média por competição, linha de cada time
separada por mando, e a sequência crua dos últimos 10 jogos. A base
jogo a jogo não está aqui.

## Por onde começar

- [`indice.json`](https://raw.githubusercontent.com/madeireirast22/resumo-futebol/main/indice.json) — a régua das competições (~30 KB).
  Leia primeiro: ele lista, por competição, o caminho exato do arquivo de
  cada time. Não adivinhe nome de arquivo, pegue dali.
- `times/<liga>-<ano>/<time>.json` — **um time, ~8 KB.** É o que você quer
  na maioria das vezes: traz as médias do time, a sequência dos últimos 10
  jogos e a régua da competição junto, então uma busca só já basta.
- `<liga>-<ano>.json` — a competição inteira (~150 a 280 KB). Use só quando
  precisar de todos os times de uma vez; para projetar um confronto, dois
  arquivos de time custam 16 KB em vez de 280.

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

## Cache

O `raw.githubusercontent.com` guarda cada arquivo por uns 5 minutos. Logo
depois de uma atualização você pode receber a versão anterior — inclusive um
`indice.json` velho, cujos caminhos de time podem não existir mais e darem
404. Como o robô publica uma vez por dia, isso quase nunca aparece; se
aparecer, espere alguns minutos.

Dado derivado da SofaScore. Aqui só entram agregados, não a base dela.
