# Spike C: WhatsApp Intake -> Structured Profile

Feasibility analysis for LLM-driven insurance data collection via WhatsApp conversation in PT Portuguese.

Research conducted 2026-02-28.

---

## Customer Profile Schemas

### Seguro Automovel (Auto Insurance)

Mandatory in Portugal under Decreto-Lei n.o 291/2007. RC minimum covers 6M EUR personal injury, 1.22M EUR property damage.

| Field                                     | Type                                       | Mandatory                    | Customer Knows?      | Notes                                                  |
| ----------------------------------------- | ------------------------------------------ | ---------------------------- | -------------------- | ------------------------------------------------------ |
| **Vehicle**                               |                                            |                              |                      |                                                        |
| Matricula (plate)                         | String (AA-00-AA)                          | Yes                          | Yes                  | Triggers VIS/DUA lookup for make/model/year/fuel/cc/kW |
| Marca                                     | Enum                                       | Yes                          | Yes                  | Often auto-filled from matricula                       |
| Modelo                                    | String                                     | Yes                          | Usually              | Can be confused with trim/version                      |
| Ano de matricula                          | Integer                                    | Yes                          | Yes                  |                                                        |
| Combustivel                               | Enum: gasolina/diesel/hibrido/eletrico/GPL | Yes                          | Yes                  |                                                        |
| Cilindrada (cc)                           | Integer                                    | Yes                          | Often not            | Found in DUA                                           |
| Potencia (kW)                             | Integer                                    | Yes                          | Rarely               | Found in DUA; many confuse CV with kW                  |
| Uso do veiculo                            | Enum: particular/profissional/misto        | Yes                          | Yes                  |                                                        |
| Valor do veiculo                          | Currency                                   | Conditional (danos proprios) | Rarely               | EUROTAX/Autovista used by insurers                     |
| Garagem / pernoita                        | Enum: garagem/rua/parque fechado           | Yes                          | Yes                  |                                                        |
| Municipio de circulacao                   | String/Enum                                | Yes                          | Yes                  |                                                        |
| **Policyholder / Driver**                 |                                            |                              |                      |                                                        |
| Nome completo                             | String                                     | Yes                          | Yes                  |                                                        |
| Data de nascimento                        | Date                                       | Yes                          | Yes                  |                                                        |
| NIF                                       | String (9 digits)                          | Yes                          | Yes                  |                                                        |
| Morada                                    | String                                     | Yes                          | Yes                  |                                                        |
| Carta de conducao — data emissao          | Date                                       | Yes                          | Often not            | Needs physical licence                                 |
| Carta de conducao — categoria             | Enum: B/B+E/etc                            | Yes                          | Usually              |                                                        |
| Condutor habitual (same as policyholder?) | Boolean                                    | Yes                          | Yes                  |                                                        |
| Condutor habitual data nascimento         | Date                                       | Conditional                  | Yes                  |                                                        |
| Condutor habitual carta emissao           | Date                                       | Conditional                  | Often not            |                                                        |
| **Claims & History**                      |                                            |                              |                      |                                                        |
| Tem seguro atual?                         | Boolean                                    | Yes                          | Yes                  |                                                        |
| Seguradora atual                          | String                                     | Conditional                  | Yes                  |                                                        |
| Classe bonus-malus                        | Integer (0-13)                             | Yes                          | Rarely               | Most don't know their class number                     |
| Sinistros ultimos 5 anos                  | Integer                                    | Yes                          | Approximately        |                                                        |
| Responsabilidade                          | Enum: culpa/sem culpa/parcial              | Conditional                  | Sometimes            |                                                        |
| **Coverage**                              |                                            |                              |                      |                                                        |
| Tipo de cobertura                         | Enum: RC/RC+incendio+roubo/danos proprios  | Yes                          | Yes (once explained) |                                                        |
| Franquia                                  | Enum/Currency                              | No                           | Rarely               |                                                        |
| Acidentes pessoais                        | Boolean                                    | No                           |                      |                                                        |
| Assistencia em viagem                     | Boolean                                    | No                           |                      |                                                        |
| Data de inicio                            | Date                                       | Yes                          | Yes                  |                                                        |

**Fields customers don't know without documents:** cilindrada, potencia (kW), classe bonus-malus, data emissao carta de conducao, valor de mercado do veiculo.

### Seguro Multirriscos Habitacao (Home Insurance)

Not mandatory by law (fire coverage in horizontal property is the only legal requirement), but banks require it for mortgaged properties.

| Field                         | Type                                            | Mandatory   | Customer Knows?                         | Notes                                        |
| ----------------------------- | ----------------------------------------------- | ----------- | --------------------------------------- | -------------------------------------------- |
| **Property**                  |                                                 |             |                                         |                                              |
| Morada completa (CP4+3)       | String                                          | Yes         | Usually (not always postal code suffix) |                                              |
| Tipo de imovel                | Enum: apartamento/moradia/outros                | Yes         | Yes                                     |                                              |
| Fracao autonoma               | Boolean/String                                  | Yes         | Usually                                 | "Andar" colloquially                         |
| Proprietario ou arrendatario  | Enum                                            | Yes         | Yes                                     |                                              |
| Destino                       | Enum: habitacao propria/arrendamento/secundaria | Yes         | Yes                                     |                                              |
| Ano de construcao             | Integer                                         | Yes         | Often vague                             | "Anos 70" — needs exact year for rating      |
| Area bruta / util (m2)        | Integer                                         | Yes         | Vague                                   | Caderneta predial vs felt area differ 20-30% |
| Tipo de construcao (paredes)  | Enum: tijolo/betao/pedra/madeira/mista          | Yes         | Often not                               | Critical for fire risk                       |
| Tipo de cobertura (telhado)   | Enum: telha/terraco/zinco/outros                | Yes         | Often not                               |                                              |
| N pisos do edificio           | Integer                                         | Yes         | Usually                                 |                                              |
| Andar da fracao               | Integer                                         | Yes         | Yes                                     |                                              |
| Zona de risco de inundacao    | Boolean                                         | Conditional | Rarely                                  | Customer usually doesn't know                |
| Chamine ou lareira            | Boolean                                         | Yes         | Yes                                     |                                              |
| Garagem                       | Boolean                                         | Yes         | Yes                                     |                                              |
| Piscina                       | Boolean                                         | Yes         | Yes                                     |                                              |
| Hipoteca / credito habitacao  | Boolean                                         | Yes         | Yes                                     | If yes, bank details required                |
| Nome do banco credor          | String                                          | Conditional | Yes                                     |                                              |
| **Insured Values**            |                                                 |             |                                         |                                              |
| Capital para o edificio (EUR) | Currency                                        | Yes         | Rarely                                  | Rebuild cost, NOT market value               |
| Capital para recheio (EUR)    | Currency                                        | Yes         | Vague                                   | Most underestimate                           |
| **Claims History**            |                                                 |             |                                         |                                              |
| Sinistros ultimos 3 anos      | Boolean                                         | Yes         | Yes                                     |                                              |
| Tipo de sinistro              | Enum                                            | Conditional | Yes                                     |                                              |

**Fields customers don't know without documents:** ano de construcao exato, area bruta (m2), tipo de construcao/material paredes, capital do edificio para reconstrucao (single hardest field — market value != rebuild cost), permilagem (apartment condominiums).

### Seguro de Saude (Health Insurance)

Most complex product. Two phases: initial estimate (age-based, 2-4 turns) and full subscription requiring QIS (Questionario Individual de Saude) per person.

| Field                            | Type                                    | Mandatory     | Customer Knows?   | Notes                      |
| -------------------------------- | --------------------------------------- | ------------- | ----------------- | -------------------------- |
| **Basic Personal (per insured)** |                                         |               |                   |                            |
| Nome completo                    | String                                  | Yes           | Yes               |                            |
| Data de nascimento               | Date                                    | Yes           | Yes               | Premium is age-band driven |
| NIF                              | String                                  | Yes           | Yes               |                            |
| Sexo                             | Enum: M/F                               | Yes           | Yes               |                            |
| Morada                           | String                                  | Yes           | Yes               |                            |
| **Policy Structure**             |                                         |               |                   |                            |
| N pessoas a segurar              | Integer                                 | Yes           | Yes               |                            |
| Relacao entre segurados          | Enum: individual/casal/familia          | Yes           | Yes               |                            |
| Idades de cada segurado          | Integer[]                               | Yes           | Yes               |                            |
| Plano desejado                   | Enum: basico/medio/completo             | Yes           | After explanation |                            |
| Periodicidade pagamento          | Enum: mensal/trimestral/semestral/anual | Yes           | Yes               |                            |
| **QIS (per insured)**            |                                         |               |                   |                            |
| Doenca cronica                   | Boolean                                 | Yes           | Yes (sensitive)   | Triggers branches          |
| Doencas declaradas               | Multi-enum/free text                    | Conditional   | Yes               |                            |
| Tratamento medico atual          | Boolean                                 | Yes           | Yes               |                            |
| Medicacao regular                | Boolean                                 | Yes           | Yes               |                            |
| Cirurgias ultimos X anos         | Boolean                                 | Yes           | Yes               |                            |
| Saude mental                     | Boolean                                 | Yes           | Very sensitive    | High drop-off risk         |
| Deficiencia/incapacidade         | Boolean                                 | Yes           | Sensitive         |                            |
| Altura e peso (IMC)              | Numeric                                 | Some insurers | Usually           |                            |
| Tabaco ultimos X anos            | Boolean                                 | Yes           | Yes               |                            |
| Alcool regular                   | Boolean                                 | Yes           | Sensitive         |                            |
| Desportos de risco               | Boolean                                 | No            | Yes               |                            |

**Regulatory notes:**
- Health data = GDPR Article 9 special category, requires explicit consent
- ASF "direito ao esquecimento" — insurers cannot use data on conditions overcome/mitigated after right-to-be-forgotten period
- QIS responses are legal declarations; false declarations can void contract (Codigo do Contrato de Seguro, Art. 24-25)
- EU AI Act Annex III, 5(b) — AI systems for life/health insurance risk assessment = high-risk

---

## Conversation Flow Analysis

### Auto — Optimal Flow (10-14 turns)

| Turn | Question                                                                   | Fields Captured                                       |
| ---- | -------------------------------------------------------------------------- | ----------------------------------------------------- |
| 1    | "Qual e a matricula do veiculo?"                                           | matricula -> VIS/DUA lookup (6 fields auto-filled)    |
| 2    | Confirm vehicle details from lookup                                        | marca, modelo, ano, combustivel, cilindrada, potencia |
| 3    | "Como usa o carro?" + "Onde fica guardado a noite?"                        | uso, garagem                                          |
| 4    | "Em que concelho circula mais?"                                            | municipio                                             |
| 5    | "Qual e a sua data de nascimento?" + "Ha quanto tempo tem carta?"          | data_nascimento, data_emissao_carta                   |
| 6    | "E o condutor habitual?" (branches if no)                                  | condutor_habitual + driver details                    |
| 7    | "Tem seguro neste momento? Com quem?"                                      | seguro_atual, seguradora                              |
| 8    | "Nos ultimos 5 anos, teve algum sinistro com responsabilidade?"            | sinistros, bonus-malus proxy                          |
| 9    | "Que tipo de cobertura procura?" (RC / RC+incendio+roubo / danos proprios) | cobertura_tipo                                        |
| 10   | "Quando quer que o seguro comece?"                                         | data_inicio                                           |

**Key design decisions:**
- Matricula first — highest single-question information yield via API lookup
- Reframe bonus-malus as "number of fault claims in 5 years" — customers don't know their class number
- Claims/coverage questions last (mildly uncomfortable, need context)

### Home — Optimal Flow (14-18 turns)

Longer than auto. Less can be auto-filled. Construction type requires guided disambiguation.

**Critical friction points:**
- **Ano de construcao:** Customers say "e dos anos 70." Bot needs to accept decade or guide toward caderneta predial.
- **Area (m2):** Caderneta predial shows gross area; customers think of usable area (15-20% less). Discrepancy affects insured capital.
- **Capital do edificio:** Biggest failure point. Rebuild cost != market value. In Lisbon, market value can be 3-5x rebuild cost. Bot cannot calculate without external data. Proxy: ~1,200-1,500 EUR/m2 for standard construction.
- **Tipo de construcao:** Simplify to 3 categories: "pedra antiga (pre-1960)", "tijolo/betao (moderna)", "madeira ou mista" instead of 7 options.

### Health — Two-Phase Flow

Phase 1 (initial estimate): 2-4 turns (number of people, ages, plan choice -> price estimate).
Phase 2 (full subscription/QIS): 8-15 additional turns per person. Family of 4 with conditions = 30-40+ turns total.

**Health is the only product where human handoff is near-certain for complex cases.**

---

## PT Portuguese Language Considerations

### Insurance Terminology (Formal vs Colloquial)

| Formal                        | What Customers Say                                           |
| ----------------------------- | ------------------------------------------------------------ |
| Seguro automovel              | "seguro do carro"                                            |
| Responsabilidade civil        | "RC" / "o seguro basico" / "quero so o obrigatorio"          |
| Contra todos os riscos        | "seguro completo" / "danos proprios"                         |
| Seguro multirriscos habitacao | "seguro de casa"                                             |
| Apolice                       | "o contrato" / "o papel do seguro"                           |
| Premio                        | "o que pago por mes" / "a anuidade"                          |
| Franquia                      | "participacao" / "o que pago se houver sinistro"             |
| Sinistro                      | "acidente" / "problema"                                      |
| Tomador                       | "cliente" / "quem paga" — formal term unknown to most        |
| DUA                           | "os papeis do carro" / "o livrete" (outdated but still used) |
| Caderneta predial             | "os papeis da casa"                                          |
| Carta de conducao             | "carta" (always shortened)                                   |

### PT-PT vs PT-BR Differences

Portugal has ~300,000+ Brazilian residents. Key risks:

| Concept                 | PT-PT             | PT-BR                   | Bot Action                        |
| ----------------------- | ----------------- | ----------------------- | --------------------------------- |
| Vehicle registration    | Matricula         | Placa                   | Map "placa" -> matricula          |
| Driving licence         | Carta de conducao | CNH                     | Different document; clarify       |
| Tax ID                  | NIF               | CPF                     | BR customers need NIF explanation |
| Comprehensive insurance | Danos proprios    | "Seguro completo/total" | Minor ambiguity                   |

**Detection signal:** If customer uses "placa", "CNH", or "CPF" -> likely Brazilian. Adapt: explain NIF requirement, clarify PT document equivalents.

### Cultural Considerations

- **Privacy:** PT customers more reserved than Brazilians about health info with institutions. Frame with RGPD assurance.
- **Distrust:** Low trust in insurance companies (Southern European pattern). Avoid sales language. Neutral, informational tone.
- **Directness:** PT-PT communication is less effusive than BR-PT. Avoid "Excelente! Muito obrigado!" — sounds condescending. Single, direct questions.
- **Age demographics:** Significant 50+ buyer segment. Less comfortable with WhatsApp-first. May type "nao percebo" or abandon silently.
- **Health sensitivity:** Mental health questions are strong drop-off triggers. Frame as clinical. Always offer phone alternative.
- **"Seguro de vida" ambiguity:** Could mean life insurance OR mortgage-linked life insurance. Always clarify immediately.

---

## Edge Cases and Failure Modes

### Documents Not at Hand (~30% of fields)

Most common operational failure mode. Strategy: maintain "partial profile" state, allow "continuar mais tarde" with session save.

| Field                           | "Don't have it" Frequency | Strategy                                                             |
| ------------------------------- | ------------------------- | -------------------------------------------------------------------- |
| Matricula                       | Rare                      | Ask to check outside or car app                                      |
| Cilindrada / potencia           | Very common               | "Pode encontrar nos papeis do carro. Quer continuar com estimativa?" |
| Bonus-malus class               | Very common               | Reframe as fault claims in 5 years                                   |
| Ano construcao                  | Common                    | Accept decade, guide to caderneta predial                            |
| Capital edificio (rebuild cost) | Almost universal          | Offer calculator based on area + construction type                   |
| Area m2                         | Common                    | Accept approximate                                                   |
| Current policy details          | Common                    | "Pode enviar foto da apolice? Consigo retirar os dados"              |

### Unusual Insurance Subjects

- **Classic car (pre-1981):** VIS database may not have it. Agreed value policy required. -> Flag for human specialist.
- **Mixed-use property:** Part residential, part commercial. Standard MR excludes commercial. -> Detect "trabalho em casa" / "tenho escritorio" -> escalate.
- **Rural property / quinta:** Agricultural land, outbuildings, equipment = different coverages. -> Human handoff.
- **Property under renovation:** Standard MR may not cover uninhabited properties. -> Ask "esta habitada?" early.
- **Pre-existing health conditions (2+):** QIS branches dramatically. -> Offer phone option.

### Multi-Product and Family Scenarios

- **Multi-product:** Complete one product before starting another. Reuse shared data (name, NIF, morada).
- **Multiple drivers on auto:** Each driver adds ~4 turns. State management complexity high.
- **Family health insurance:** Medis allows up to 8 people. Full QIS per person. Family of 4 = 30-40+ turns.

### Sensitive Disclosures

- **Accident fault:** Frame as factual, not judgmental. "Houve algum sinistro em que o seguro pagou por responsabilidade sua?"
- **Health conditions:** Introduce QIS with framing: "Estas perguntas sao obrigatorias por lei. Sao confidenciais." Respond neutrally after each disclosure.
- **Mental health:** Hardest to collect. Always offer "prefere falar com alguem por telefone?"

---

## Feasibility Assessment

### LLM Automation Confidence

| Product | Quote Generation       | Full Subscription | Assessment                                              |
| ------- | ---------------------- | ----------------- | ------------------------------------------------------- |
| Auto    | High                   | Medium-high       | Best candidate for full automation                      |
| Home    | Medium-high            | Medium            | Blocked by rebuild cost estimation                      |
| Health  | Medium (estimate only) | Low-medium        | QIS branching + sensitivity makes full automation risky |

### LLM Strengths

- Natural disambiguation ("o meu carro e um Golf" -> Volkswagen Golf)
- Grouping related questions naturally
- PT/BR code-switching (modern LLMs handle well)
- Recovering from ambiguous answers ("talvez 2015 ou 2016" -> store as range)
- Document image processing (photo of DUA, apolice, caderneta predial -> extract fields)

### LLM Weaknesses (Highest Risk)

1. **Rebuild cost estimation (home):** No reliable way to extract defensible rebuild value from conversation alone. Needs external calculator integration.
2. **Bonus-malus verification:** Can collect proxy but cannot verify. Previous policy document required by insurer.
3. **Health QIS branching fidelity:** 10+ conditional branches per person, 8 possible insured. Miscaptured condition = legal exposure.
4. **Document dependency:** ~30% of fields are on documents customer may not have. Without photo-to-field extraction, these become blockers.
5. **Vehicle spec hallucination:** Older/unusual vehicles may not be in training data. Must enforce "nao sei" paths with escalation.

### Expected Conversation Length

| Product                                 | Best Case | Typical | Complex                        |
| --------------------------------------- | --------- | ------- | ------------------------------ |
| Auto                                    | 10 msgs   | 14-16   | 20-25 (multi-driver)           |
| Home                                    | 14 msgs   | 18-22   | 28-35 (mortgage, missing docs) |
| Health (individual, no conditions)      | 12 msgs   | 16-18   | N/A                            |
| Health (family of 4, 1 with conditions) | —         | 28-35   | 40-55                          |

### Human Handoff Rates

| Scenario                           | Handoff Rate | Triggers                             |
| ---------------------------------- | ------------ | ------------------------------------ |
| Auto — standard, clean history     | 5-10%        | Plate not found, classic vehicle     |
| Auto — multi-driver, claims        | 15-20%       | Complex history, fault disputes      |
| Home — standard apartment          | 15-20%       | Unknown construction, rebuild cost   |
| Home — moradia, quinta, unusual    | 40-50%       | Agricultural use, vacant, renovation |
| Health — individual, no conditions | 10-15%       | QIS edge cases                       |
| Health — individual, 2+ conditions | 50-70%       | Complex underwriting                 |
| Health — family 4+                 | 30-45%       | Cumulative QIS complexity            |
| **Overall weighted average**       | **~25-30%**  |                                      |

### Biggest Risks (Ranked by Impact x Probability)

1. **Legal exposure from QIS errors (health)** — HIGH impact, MEDIUM probability. Incomplete QIS -> insurer can void contract at claim time. Mitigation: structured multi-choice, explicit confirmation per question, full transcript as ASF audit.
2. **Rebuild cost underinsurance (home)** — HIGH impact, HIGH probability. Most PT households underinsured. Mitigation: mandatory rebuild cost calculator before generating quote.
3. **PT-BR customer without NIF** — MEDIUM impact, HIGH frequency. No NIF = no contract. Mitigation: detect early, explain how to obtain, hold proposal.
4. **Document dependency breaks flow** — MEDIUM impact, HIGH frequency. Mitigation: session persistence, "continuar mais tarde", photo-to-field extraction from day one.
5. **EU AI Act high-risk classification (health)** — HIGH impact, MEDIUM probability. Aug 2026 deadline. Mitigation: LLM collects/structures data but pricing decision explicitly made by insurer's system. Log everything.

---

## MVP Recommendation

Start with **auto insurance only**. Highest LLM automation potential, clearest field structure, best API lookup opportunities (matricula -> VIS/DUA auto-fills ~6 fields), lowest regulatory risk in collection. Home second (needs rebuild cost calculator infrastructure). Health last (QIS complexity + GDPR Art. 9 + EU AI Act).

---

Sources: ASF consumer pages, MUDEY seguro carro flow, Fidelidade/Medis/Multicare simulators, Mapfre PT simulator, Seguro Directo bonus-malus explainer, Expatica PT car insurance guide, DECO seguro vida credito habitacao, EXS Seguros pre-existing conditions guide, IBA right-to-be-forgotten Portugal, Blue Arrow EU AI Act and insurance.
