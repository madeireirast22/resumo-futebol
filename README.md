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

**Olhe o campo `periodo` antes de usar a régua de uma competição.** A
contagem de jogos sozinha engana. Champions e Europa League de temporada em
curso costumam ter só as ELIMINATÓRIAS PRÉVIAS (julho e agosto), disputadas
por clubes de Malta, San Marino, Bielorrússia — a fase de liga só estreia em
meados de setembro. Se `periodo.ate` for anterior a setembro, o que está ali
não é Champions no sentido que interessa. Use a temporada anterior.

- Brasileirão A e B sustentam projeção de confronto (24 a 38 jogos por time).
- Copas não: por time são 2 a 6 jogos. Ali use só a régua da competição.
- Série B 2025 está incompleta, e o que falta não é aleatório: é o returno
  sem o turno.
- Não há jogos futuros nem odds.

## Scout por jogador

Dois níveis, listados em `indice.json` → `scout[]`:

- `scout-<liga>-<ano>.json` (60 a 140 KB) — o **ranking** da competição,
  só quem passa do piso de minutos (`pisoDeMinutos`). Por jogador: chutes
  e no gol (HT/FT e por 90), xG, **gols, gols no 1º tempo, assistências,
  cartões amarelos (FT e HT), vermelhos**, desarmes, faltas, faltas
  sofridas, defesas, posição.
- `scout-times/<liga>-<ano>/<time>.json` (10 a 60 KB) — **o elenco inteiro
  de um time, sem piso**, com os mesmos totais, **separados por casa/fora**,
  por-90 já calculado, e os **últimos 5 jogos de cada jogador** (chutes,
  no gol, gol, assistência, cartão, desarme, falta, minutos, adversário,
  mando). O `<time>` é o mesmo apelido de `times/`: pegue em
  `indice.json` → `competicoes[].times`.

Para projetar um jogador num jogo: arquivo do time dele. Para achar quem
mais chuta na liga: o ranking. Cada arquivo traz `legenda` com os campos.

`cartoes: false` (Dinamarca, Noruega, Suécia) = a base não tem cartão ali,
e `car` vem `null`. Em toda outra competição, inclusive Libertadores e
Sul-Americana, `car` é o número real.

## Próximos jogos e escalações

- `proximos.json` (~40 KB) — os jogos dos **próximos 7 dias** em todas as
  competições em curso: data, times, id do evento, e as flags `temProvavel` e
  `temConfirmada`. Regenerado às 08:00 e 23:30.
- `escalacoes/<eventoId>.json` (~6 KB) — um por jogo. Traz:
  - `provavelCasa` / `provavelFora`: os **11 com mais minutos nos últimos 3
    jogos** do time (`baseadoEm` diz quais), mais 7 do banco. É o provável
    titular, calculado do scout — âncora até sair a oficial.
  - `confirmada`: a **escalação oficial**, quando existe. A SofaScore publica
    ~1 h antes do apito; um vigia busca a cada 15 min e publica na hora. Tem
    formação, titulares, banco e desfalques. `null` = ainda não saiu.

Fluxo para um jogo de hoje: `proximos.json` → acha o `eventoId` → puxa
`escalacoes/<eventoId>.json`. Se `confirmada` for `null` e faltar mais de 1 h,
use `provavelCasa`/`provavelFora` e diga que é provável, não oficial.

## Cache

O `raw.githubusercontent.com` guarda cada arquivo por uns 5 minutos. Logo
depois de uma atualização você pode receber a versão anterior — inclusive um
`indice.json` velho, cujos caminhos de time podem não existir mais e darem
404. Como o robô publica uma vez por dia, isso quase nunca aparece; se
aparecer, espere alguns minutos.

Dado derivado da SofaScore. Aqui só entram agregados, não a base dela.
