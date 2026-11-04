<p align="center"><img src="docs/icone.png" width="96" alt="Ícone do José"></p>

<h1 align="center">José</h1>

<p align="center">Jogo de cartas de memória para 2 a 4 pessoas, cada uma no próprio celular, pela internet ou pela rede local.</p>

<p align="center">
  <a href="https://play.google.com/store/apps/details?id=com.abisay.jose"><img src="https://img.shields.io/badge/Google_Play-dispon%C3%ADvel-0B0E14?style=for-the-badge&logo=googleplay&logoColor=C05B1E" alt="Disponível no Google Play"></a>
  <img src="https://img.shields.io/badge/App_Store-em_revis%C3%A3o-0B0E14?style=for-the-badge&logo=apple&logoColor=EEEDE9" alt="App Store: em revisão">
</p>

<p align="center"><img src="docs/capa.jpg" width="720" alt="Arte de destaque do José"></p>

## Sobre

Cada jogador recebe de 4 a 6 cartas viradas para baixo e não pode olhar. Na sua vez, compra do monte ou do centro e decide se fica com a carta ou descarta. As cartas de 2 a 9 têm poderes: dar cartas a alguém, embaralhar a mão de outro jogador, espiar ou trocar. Quem acha que tem a menor soma grita "JOSÉ" e encerra a rodada.

A sala pode ser online, com um código, ou na rede local e no hotspot do celular, sem internet. O jogo tem tutorial, modo hardcore, tempo por jogada e uma mesa que funciona com o celular em pé ou deitado. Não precisa criar conta.

## Telas

| | | | | | |
|:-:|:-:|:-:|:-:|:-:|:-:|
| <img src="docs/1-menu.jpg" width="130"> | <img src="docs/2-criar-sala.jpg" width="130"> | <img src="docs/3-sala.jpg" width="130"> | <img src="docs/4-mesa.jpg" width="130"> | <img src="docs/5-carta-comprada.jpg" width="130"> | <img src="docs/6-cola.jpg" width="130"> |

## Como foi feito

- O motor do jogo é TypeScript puro e roda no celular de quem criou a sala. Esse aparelho decide o resultado de cada jogada; os outros mandam intenções e desenham o que recebem.
- Cada jogador recebe só a parte do estado que pode ver. A tela do próprio anfitrião também usa apenas a visão dele.
- Quando duas pessoas gritam "José" quase ao mesmo tempo, o app compensa a latência e a diferença de relógio de cada aparelho com uma medição no estilo NTP. Os gritos são reunidos numa janela curta, e um sorteio desempata os que chegam a menos de 30 ms um do outro.
- A mesma partida roda sobre TCP na rede local, com um módulo nativo em Swift e Kotlin, ou sobre o Supabase Realtime Broadcast pela internet. O código da sala indica qual dos dois usar.
- Os aparelhos mandam sinal de vida a cada 3 s, e quem cai tem um tempo para voltar. O estado é salvo depois de cada jogada, então a partida continua se o app fechar.
- O embaralhamento recebe uma semente, o que torna as partidas reproduzíveis nos testes com Jest e Testing Library. Os fluxos E2E rodam em Maestro.

## Arquitetura

```mermaid
flowchart LR
  subgraph Anfitriao["Celular do anfitrião"]
    Motor["Motor do jogo (TS puro)"] --> Visao["Visão filtrada por jogador"]
    Motor --> Snap["Estado salvo em disco"]
  end
  Visao -- "TCP na rede local" --> C1["Convidado"]
  Visao -- "Supabase Realtime Broadcast" --> C2["Convidado"]
  C1 -- "intenções" --> Motor
  C2 -- "intenções" --> Motor
```

## Stack

Expo SDK 58 · React Native 0.88 · React 19 · TypeScript · Reanimated 4 · Supabase Realtime · react-native-tcp-socket · módulo nativo (Swift/Kotlin) · Jest · Maestro · EAS Build/Update

## Meu papel

Criei as regras do jogo, a interface, o app e a camada de rede, e publiquei nas lojas.

<sub>O código-fonte é privado. Para ver uma demo ou conversar sobre o projeto, fale comigo em <a href="https://abisay.tech">abisay.tech</a> ou no <a href="https://www.linkedin.com/in/jahncarlos">LinkedIn</a>.</sub>
