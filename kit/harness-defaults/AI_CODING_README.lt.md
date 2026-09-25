# AI kodavimo pastabos

Trumpos pastabos tiems, kas dirba šioje repozitorijoje po to, kai ji buvo paruošta agentiniam kodavimui. **Agentai skaito `AGENTS.md`.** Šis failas — tau.

**Pradėk nuo šito (būtina):** įvadai ir mokymai yra `AI_CODING_LEARN.md`. Peržiūrėk / perskaityk juos prieš laikydamas agentą jau pažįstamu procesu.

> Neperskaitytas skill'as — tik ilgesnis promptas, kurio nekontroliuoji. Vis tiek juos perskaityk.

Komandos darbo eigos skill'ai: [github.com/coleam00/skills](https://github.com/coleam00/skills). Įdiegtos kopijos: `.agents/skills/<name>/SKILL.md`.

## Kodėl Cole ir Addy

Du katalogai, dvi funkcijos. Jie veikia vienas šalia kito — ne kaip antras procesas.

**Cole** ([coleam00/skills](https://github.com/coleam00/skills)) yra operacinė sistema. Jis sprendžia, **kada** darbas atliekamas ir **kokia eilės tvarka**: produkto tikslas (`plan-create-prd`), požiūris (`plan-architecture`), ticket'ai (`plan-create-stories`), tada PIV ciklas (prime → plan → implement → validate → review → commit → PR).

**Addy** ([addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)) yra vyresniojo inžinieriaus vadovas. Jis sprendžia, **kaip gerai** atliekama darbo dalis konkrečioje srityje — UI, API, saugumas, našumas, supaprastinimas, pirminio šaltinio dokumentacija, patikra naršyklėje, išleidimas ir visa kita iš atrinkto rinkinio. Įgyvendindamas perskaityk atitinkamą `SKILL.md`.

Cole'o PRD skill'as inžinerinius sprendimus (biblioteka ir versija, duomenų modelis, saugumo ribos, testavimo architektūra, projekto struktūra) net perkelia į „Osmani sąrašą“, kurio vieta yra architektūroje / specifikacijoje, o ne PRD. Toks ir yra numatytas pasidalijimas: Cole nustato darbų eiliškumą; Addy kelia kartelę kiekvieno žingsnio viduje.

Toliau esantis darbo eigos skyrius yra veikiantis numatytasis variantas. Vėliau jį sugriežtink, jei komanda norės kitokio proceso.

## Prisijungimas prie šios paruoštos repozitorijos (nauja mašina / naujas komandos narys)

Į git įtraukti failai (`AGENTS.md`, pagalbiniai failai, `skills-lock.json`, native taisyklės, ši atmintinė) yra komandos darbo eiga. Negeneruok jų iš naujo. Šiai mašinai vis tiek reikia:

1. Atkurti skill'us (jie yra gitignore sąraše; `skills-lock.json` yra įtrauktas į git):

```bash
npx skills experimental_install --yes
```

Reikia interneto. Vykdyk iš repozitorijos šaknies.

2. Atidaryk projektą Cursor. Įdiek trūkstamus marketplace papildinius (`/add-plugin …` žemiau), perkrauk ir prijunk Context7 / Sonatype raktus per Customize. Niekada nekelk raktų į git.

3. Perskaityk šį failą ir `AI_CODING_LEARN.md`, prieš laikydamas agentą jau pažįstamu procesu.

Rezultatas toks pat, jei įklijuoji rinkinio GitHub README promptą: agentas privalo klasifikuoti tai kaip **prisijungimą** (`join`) ir sustoti po šios mašinos paruošimo (`harness`). Neatsisiųsk `kit/` vien dėl prisijungimo.

## Darbo eiga

**prime → plan → implement → validate → review → commit → PR**

Aplink šį ciklą yra dalys, kurios jį pamaitina (PRD, architektūra, epikų skaidymas), dalys, kurios jį vykdo paraleliai (worktrees), ir meta-skill'ai, leidžiantys susikurti daugiau savo AI sluoksnio (taisyklės, hooks, skill'ai, galimybių peržiūros).

**Paprasta** — rašybos klaida, pervadinimas, akivaizdus vieno failo pataisymas:

specifikacija pokalbyje → įgyvendinimas → patikra (test / lint / typecheck; naršyklė, jei UI)

**Pilna** — funkcija, ticket'as, netrivialus pakeitimas:

specifikacija → darbų sąrašas → įgyvendinimas → patikra

Tai atitinka coleam00 skill'us (nekurk paralelaus proceso). Čia tik pavadinimai — prieš pasikliaudamas atidaryk kiekvieną `SKILL.md` ([coleam00/skills](https://github.com/coleam00/skills) arba `.agents/skills/<name>/SKILL.md`):

| Tipas                | Skill'ai                                                                                                                                         |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Produkto tikslas     | `plan-create-prd`                                                                                                                                |
| Požiūris / stack'as  | `plan-architecture`                                                                                                                              |
| Ticket'ai            | `plan-create-stories`                                                                                                                            |
| Funkcija / ticket'as | `prime-codebase` (arba `prime-frontend` / `prime-backend`) → `piv-plan-implementation` → `piv-implement` → `piv-validate` → `piv-review-changes` |
| Klaida su issue      | `piv-investigate-issue` → `piv-implement-issue`                                                                                                  |
| Commit / PR          | `piv-commit` / `piv-create-pr` tik to ciklo viduje arba kai paprašai                                                                             |

Mažam / mechaniškam darbui PIV ciklą praleisk. Įgyvendinimo metu: chirurgiškai tikslūs diff'ai (`karpathy-guidelines`) ir atitinkamas Addy skill'as, kai darbo dalis yra UI, API, saugumas, našumas ar išleidimas. Projekto komandos (dev, test, lint, build) yra `AGENTS.md`.

## Skill'ai

Vykdyk iš **repozitorijos šaknies**. Įdiegimai patenka į `.agents/skills/` (gitignore sąraše). Po pridėjimo / atnaujinimo įtrauk į git `skills-lock.json`.

| Ką                                                                    | Komanda                                 |
| --------------------------------------------------------------------- | --------------------------------------- |
| Atkurti iš lock failo (klonavimas, nauja mašina, ištrintas katalogas) | `npx skills experimental_install --yes` |
| Pažiūrėti, ar įdiegti skill'ai turi atnaujinimų                       | `npx skills check`                      |
| Pritaikyti atnaujinimus                                               | `npx skills update --yes`               |
| Parodyti, kas įdiegta                                                 | `npx skills list`                       |

**Numatytieji šaltiniai** (jau yra `skills-lock.json`). Kasdienis atkūrimas vyksta iš lock failo. `add` naudok tada, kai tose GitHub repozitorijose atsirado skill'ų, kurių šiame lock faile dar nėra:

```bash
npx skills add coleam00/skills --yes
npx skills add forrestchang/andrej-karpathy-skills --yes
npx skills add emilkowalski/skills --yes
npx skills add mattpocock/skills --skill improve-codebase-architecture research codebase-design --yes
npx skills add addyosmani/agent-skills --yes \
  -s frontend-ui-engineering \
  -s api-and-interface-design \
  -s security-and-hardening \
  -s performance-optimization \
  -s code-simplification \
  -s source-driven-development \
  -s browser-testing-with-devtools \
  -s debugging-and-error-recovery \
  -s observability-and-instrumentation \
  -s ci-cd-and-automation \
  -s documentation-and-adrs \
  -s shipping-and-launch \
  -s constraint-driven-development \
  -s deprecation-and-migration
npx skills add addyosmani/agent-skills --yes \
  -s idea-refine \
  -s code-review-and-quality
```

Matt Pocock: **tik tie trys** skill'ai, ne visas katalogas. Tada įtrauk į git atnaujintą `skills-lock.json`.

Naršyk daugiau: [skills.sh](https://skills.sh/). Paieška: `npx skills find [query]`.

## Cursor papildiniai (vieną kartą kiekvienoje mašinoje)

Numatytieji MCP / marketplace papildiniai. Įdiegimo CLI nėra. Jei papildinio nėra, įklijuok Cursor pokalbyje ir perkrauk:

```
/add-plugin cursor-team-kit
/add-plugin context7-plugin
/add-plugin sonatype-cursor-plugin
/add-plugin modern-web-guidance
```

- Cursor Team Kit — `cursor-team-kit`
- Context7 — `context7-plugin`
- Sonatype — `sonatype-cursor-plugin`
- Modern Web Guidance — `modern-web-guidance`

Context7 ir Sonatype prijunk per Customize. `"key": true` nustatymuose reiškia „prijunk per UI“, o ne „įkelk paslaptį į git“.

## Įrankiai, kuriuos agentas turi naudoti

- Bibliotekų / API dokumentacija: Context7 MCP (ne apmokymo atmintis ir ne bendra paieška internete)
- Nauji ar atnaujinami paketai: Sonatype MCP prieš versijos fiksavimą
- GitHub: tik `gh` CLI (jokio GitHub MCP)

## Neįtrauk į git

- Papildinių API raktų, `.env`, prisijungimo duomenų
- `.agents/skills/` — atkuriama su `npx skills experimental_install --yes`

## Žemėlapis

| Kelias                                     | Vaidmuo                                                                                       |
| ------------------------------------------ | --------------------------------------------------------------------------------------------- |
| `AGENTS.md`                                | Visada įkeliama agentų ašis (`spine`); `Pointers` nurodo, kada perskaityti pagalbinius failus |
| `docs/agents/`                             | Gylis pagal poreikį (stack'as, konvencijos, UI, testai)                                       |
| `skills-lock.json`                         | Fiksuoti skill'ų šaltiniai — šį failą įtrauk į git                                            |
| `.cursor/rules/ai-coding-native-rules.mdc` | Cursor namų darbo eiga + įrankiai, kiekviename pokalbyje (ne projekto glob'ai)                |
| `AI_CODING_LEARN.md`                       | Būtini įvadai ir mokymai                                                                      |
| Šis failas                                 | Žmonėms skirta atmintinė                                                                      |
