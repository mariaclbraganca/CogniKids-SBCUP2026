# Design do front-end — CogniKids Fase 2

Documentação de UI/UX do novo front-end. Os arquivos `.html` são
autocontidos: abra qualquer um no navegador, sem servidor nem build.

| Arquivo | Conteúdo |
|---|---|
| [`01-fundacao-frontend.html`](01-fundacao-frontend.html) | Stack (React+TS+Vite), design system, princípios DAD, evidência de acessibilidade cognitiva |
| [`02-jornadas-3-perfis.html`](02-jornadas-3-perfis.html) | Wireframes tela a tela das jornadas de responsável, aluno e professor, com endpoints e estados de exceção |
| [`03-prototipos-telas.html`](03-prototipos-telas.html) | Protótipos de alta fidelidade (HTML/CSS reais) e o logo redesenhado |
| [`logo-cognikids.svg`](logo-cognikids.svg) | Logo em vetor, paleta acessível |

Versões hospedadas (mesmo conteúdo, exigem login na conta que publicou):

- Fundação — https://claude.ai/code/artifact/47dda50b-3381-4c90-93c1-a8dbb0a7bfa4
- Jornadas — https://claude.ai/code/artifact/870b37c5-879d-4ec9-9bf3-73a47c2923e2
- Protótipos — https://claude.ai/code/artifact/08df4d50-9c94-4c87-8f46-e676093d93cb

## Decisões que governam o design

**Adaptação por necessidades funcionais, não por diagnóstico.** O responsável
responde 12 perguntas sobre comportamento observável ("ela se incomoda com
sons altos?"), nunca sobre laudo. Diagnóstico não determina necessidade de
interface, comorbidade é a regra, e diagnóstico é dado sensível de menor
(LGPD art. 11).

**Conflito entre necessidades resolve para o mais protetivo.** Criança que
evita estímulo e também se desengaja de tela neutra recebe `calmo`.

**Framework DAD (Data, Autonomy, Dignity)** como critério de aceitação de
cada tela. A tela inicial do aluno são ações que ele pode tomar, não um
relatório sobre ele. Dado biométrico bruto nunca aparece para a criança.

## Paleta

Derivada das matizes do logo original, dessaturada até passar em WCAG AA.
Contraste verificado nos temas claro e escuro.

| Papel | Claro | Escuro | Contraste |
|---|---|---|---|
| Marca / aluno | `#5B4B9E` | `#B3A6F2` | 6,8:1 · 8,4:1 |
| Professor / estável | `#1D6E77` | `#6FC5CE` | 5,7:1 · 9,1:1 |
| Responsável / atenção | `#9A5B12` | `#E0A863` | 5,2:1 · 8,6:1 |
| Prioritário | `#9B3268` | `#EE93BC` | 6,6:1 · 8,3:1 |

## Pendências

- **Login acessível do aluno** — e-mail e senha exclui criança em
  alfabetização. Proposta de login por sequência de figuras está no
  documento 03. Exige rota nova no backend.
- **Pausa offline** — fila local no PWA; o Wi-Fi da escola cai justamente
  quando a criança mais precisa.
- **Assentimento da criança** — além do consentimento do responsável,
  padrão CEP/CONEP para pesquisa com menores.
