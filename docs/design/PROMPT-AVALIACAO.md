# Prompt para avaliação externa dos artefatos de design

Copie o texto abaixo (entre as linhas) para um chat novo no claude.ai e
**anexe os três arquivos**: `01-fundacao-frontend.html`,
`02-jornadas-3-perfis.html` e `03-prototipos-telas.html`.

---

Você é revisor externo de um projeto de pesquisa aplicada. Quero uma
avaliação **crítica e adversarial** — não quero validação nem elogios.
Assuma que o material tem falhas e sua tarefa é encontrá-las.

## Contexto

**CogniKids** é um sistema de IoT + Machine Learning para apoiar estudantes
neurodivergentes em escolas inclusivas. Artigo aceito no SBCUP 2026 (Simpósio
Brasileiro de Computação Ubíqua e Pervasiva), ainda não publicado. Fase 1
(prova de conceito com dados sintéticos) concluída; Fase 2 em construção.

**Arquitetura em 4 camadas:**
1. Pulseira M5StickC mede BPM, GSR e movimento → MQTT
2. Ponte MQTT → Redis (buffer)
3. Worker com Random Forest classifica desregulação emocional → MongoDB
4. Aplicação: dashboard para professor e responsável

**Estado do backend (já implementado e testado):**
- 125 rotas REST em Flask, 182 testes automatizados passando
- Autorização centralizada: professor só acessa alunos das próprias turmas
- **Consentimento granular (LGPD):** 4 finalidades independentes e
  revogáveis (usar app, coletar biometria, compartilhar com escola, usar em
  pesquisa). Nega por padrão. Revogação bloqueia a ingestão de dados
  imediatamente, tanto na API quanto na ponte MQTT. Cada mudança é auditada.
- **Perfil funcional de acessibilidade:** o responsável responde 12 perguntas
  sobre comportamento observável ("ela se incomoda com sons altos?"),
  **nunca sobre diagnóstico**. As respostas viram tokens de interface
  (estímulo, densidade, texto, alvo de toque, etc.). Quando duas
  necessidades pedem valores opostos, vence o mais protetivo.
- **Pedido de pausa:** o aluno sinaliza que precisa de um tempo; as
  estratégias vêm do kit de apoio preenchido pela família e são ordenadas
  pelo que já funcionou para aquela criança.

**Limites conhecidos e assumidos:**
- O modelo de ML foi treinado **apenas com dados sintéticos de literatura de
  TEA**. A interface cobre neurodivergência ampla; o classificador não.
- A pulseira não envia GSR, então o modelo opera em produção com uma das
  três features sempre nula.
- As métricas publicadas no artigo (85% acurácia, 84% recall) não são
  reprodutíveis: o script de treino não fixa sementes aleatórias. Medições
  repetidas dão 82,9% ± 1%.

## O que anexei

Três documentos de design do novo front-end (React + TypeScript + Vite):
1. **Fundação** — stack, design system, princípios, pesquisa de acessibilidade
2. **Jornadas** — wireframes tela a tela dos 3 perfis, com estados de exceção
3. **Protótipos** — telas em alta fidelidade e o logo redesenhado

## O que eu quero de você

Faça **duas avaliações separadas**, nesta ordem:

### Parte 1 — Crítica adversarial

Procure especificamente:

- **Erros factuais.** Alguma afirmação sobre WCAG, LGPD, acessibilidade
  cognitiva ou neurodivergência está incorreta ou mal fundamentada? Cite a
  passagem e explique o erro.
- **Decisões frágeis.** Onde o raciocínio não se sustenta? Que decisão de
  design foi apresentada como óbvia mas tem alternativa melhor?
- **Riscos éticos não tratados.** O que pode dar errado com crianças reais
  numa escola real que os documentos não anteciparam?
- **Omissões.** O que uma equipe de produto experiente incluiria e não está
  ali?
- **Autoengano.** Onde o material parece estar se convencendo de que resolveu
  um problema que na verdade só reformulou?

Um ponto para escrutínio especial: a decisão de **não coletar diagnóstico** e
adaptar a interface por necessidades funcionais. É defensável ou é uma
racionalização conveniente que perde informação clínica útil? Argumente
contra ela com o melhor caso possível.

### Parte 2 — Avaliação de UX e produto

- As telas funcionariam numa escola pública brasileira real, com professor
  gerenciando 30 alunos e infraestrutura precária?
- Há problemas de usabilidade que só apareceriam em uso, não no wireframe?
- A adesão dos 3 perfis é plausível? O que faria cada um abandonar o app?
- A carga de trabalho imposta ao professor é realista?
- O onboarding do responsável (consentimento + 12 perguntas + kit de apoio)
  é longo demais? Quantos desistiriam no meio?

## Formato da resposta

- Seja específico: cite trechos, nomeie telas, aponte linhas de raciocínio.
- Ordene por gravidade, do mais sério ao menos.
- Para cada problema, diga o que faria diferente.
- Se algo estiver genuinamente bom, diga em uma linha e siga — não gaste
  espaço com elogios.
- Ao final, responda: **se você fosse revisor do SBCUP e este material fosse
  submetido como contribuição de Fase 2, aceitaria, pediria revisão ou
  rejeitaria?** Justifique.

Não amenize. Prefiro descobrir os problemas agora do que numa banca ou numa
sala de aula com crianças reais.
