<div align="center">

# CogniKids

**Sinergia entre IoT e Inteligência Artificial para o Suporte Colaborativo Família-Escola na Educação Inclusiva**

[![License: PolyForm NC 1.0.0](https://img.shields.io/badge/License-PolyForm%20NC%201.0.0-blue.svg)](https://polyformproject.org/licenses/noncommercial/1.0.0/)
[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED.svg?logo=docker&logoColor=white)](https://www.docker.com/)
[![MQTT](https://img.shields.io/badge/MQTT-Mosquitto-660066.svg)](https://mosquitto.org/)
[![Redis](https://img.shields.io/badge/Redis-DC382D.svg?logo=redis&logoColor=white)](https://redis.io/)
[![MongoDB](https://img.shields.io/badge/MongoDB-47A248.svg?logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Status](https://img.shields.io/badge/Status-Fase%201%20%7C%20PoC%20conclu%C3%ADda-orange.svg)](.)

</div>

---

Este repositório guarda o código-fonte da Fase 1 do CogniKids, uma prova de conceito de plataforma educacional e terapêutica para crianças neurodivergentes (TEA, TDAH e Dislexia) que conecta professor, família e aluno em torno de dados reais de comportamento e aprendizagem.

A Fase 1 foi **concluída, validada tecnicamente e reconhecida com 3º Melhor Artigo no SBCUP 2026** (taxa de aceitação de 48,6%). O repositório está preservado como estava na submissão do artigo, porque mexer no código agora alteraria o que foi avaliado pelos revisores, e isso não faz sentido. As evoluções do projeto estão acontecendo no [repositório da Fase 2](https://github.com/mariaclbraganca/cognikids-motor-curricular), em paralelo, enquanto a validação com crianças reais aguarda aprovação do Comitê de Ética (CEP/CONEP).

---

## O problema que motivou o projeto

O professor escreve uma única atividade para a turma inteira, mas cada criança processa informação de um jeito diferente. Uma criança com TEA pode entrar em crise de desregulação emocional antes que o professor perceba que algo está errado, porque os sinais são sutis, acontecem rápido e o professor está atendendo 30 alunos ao mesmo tempo.

O que o CogniKids tenta fazer é detectar esses sinais antes que a crise se instale, usando sensores biométricos vestíveis e um modelo de ML, e entregar esse sinal ao professor com latência baixa o suficiente para que uma intervenção ainda seja possível.

---

## O que foi validado na Fase 1

| Métrica | Resultado |
|--------|-----------|
| Acurácia do modelo | 85% |
| Recall (crises detectadas) | 84% |
| Latência média do backend | 50,27 ms |
| Volume de dados testados | 50.000+ registros sintéticos |

Os dados são sintéticos porque validar com crianças reais requer aprovação ética prévia, e isso é exatamente o que está em andamento para a Fase 2. O modelo foi treinado com dados gerados a partir de literatura médica sobre padrões biométricos em crianças com TEA.

---

## Arquitetura

O sistema foi dividido em quatro camadas desacopladas, cada uma com responsabilidade única, para que uma falha em qualquer parte não derrube o pipeline inteiro:

```
Pulseira M5StickC (BPM + GSR + Acelerômetro)
        |
        | MQTT/TLS (Mosquitto)
        ↓
Bridge Python → Redis (buffer elástico)
        |
        ↓
Worker Python + Random Forest (scikit-learn) → MongoDB
        |
        ↓
Dashboard Streamlit (alertas em tempo real + relatórios)
```

A decisão de usar Redis como buffer entre a ingestão MQTT e o processamento ML não foi estética: ela desacopla os dois fluxos de forma que picos de leitura dos sensores não afetem a latência das predições, e o sistema continua funcionando se o worker cair e voltar depois.

---

## Estrutura do repositório

```
cognikids-backend/          ← API Python (Flask), modelos, controllers, testes
cognikids-front-end/        ← Dashboard Streamlit
cognikids-pulseira-m5stack/ ← Firmware MicroPython para o wearable
cognikids-adapt/            ← Motor de Adaptação Curricular (Fase 2, em desenvolvimento)
docs/                       ← Arquitetura, ADRs, roadmap e documentação CRISP-DM
```

---

## Considerações éticas

O projeto lida com dados de crianças e com sinais biométricos, dois contextos que exigem cuidado além do técnico. Toda a validação da Fase 1 foi feita com dados sintéticos por isso: não é possível coletar dados reais de crianças sem aprovação prévia de Comitê de Ética, e aplicar o sistema sem essa aprovação seria irresponsável, independentemente de quão bem o modelo performa.

A Fase 2 está condicionada a essa aprovação. Enquanto isso, o desenvolvimento do Motor de Adaptação Curricular avança em paralelo com datasets públicos de comportamento educacional (ASSISTments, EdNet), porque esses dados já são anonimizados e autorizados para pesquisa.

---

## Como reproduzir

```bash
git clone https://github.com/mariaclbraganca/CogniKids-SBCUP2026
cd CogniKids-SBCUP2026
cp .env.example .env
docker-compose up
```

Os scripts de seed para popular o banco com dados sintéticos estão em `cognikids-backend/scripts/seeds/`.

---

## Autores

**Maria Clara Ribeiro Di Bragança** — pesquisadora principal, desenvolvimento e validação
**Orientação:** SENAI FATESG, Goiânia, GO

Artigo publicado nos *Anais do SBCUP 2026*. Para citar o projeto, consulte o artigo completo.

---

> A Fase 2 está em desenvolvimento em [cognikids-motor-curricular](https://github.com/mariaclbraganca/cognikids-motor-curricular).
