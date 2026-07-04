# Universal Independency Declaration

> **DRAFT — wymaga akceptacji DAO.** Wszystkie parametry w tym repozytorium mają
> status *propozycji roboczej* i wchodzą w życie dopiero po przegłosowaniu przez
> IPI DAO (proces on-chain governance). Do tego czasu należy je traktować jako
> materiał do dyskusji, a nie jako obowiązujące ustawienia sieci.

**Independent Protocol Infrastructure (IPI)** to federacja rozproszonych,
niezależnych organizacji, zjednoczonych wspólnym nośnikiem konsensusu —
łańcuchem opartym o **Cosmos SDK + CosmWasm**. Niniejsze repozytorium pełni rolę
**manifestu** całej sieci: opisuje deklarację niezależności protokołu, parametry
startowe łańcucha (genesis), tokenomikę oraz model federacyjnego zarządzania
DAO. Stanowi wiarygodny punkt odniesienia dla pozostałych repozytoriów
organizacji i dla społeczności.

- Strona projektu: <https://ipi.io/>
- Licencja treści manifestu: `CC-BY-ND-4.0`
- Roadmapa (6 fal, F0–F5): [#1](https://github.com/ipicoin/universal-independency-declaration/issues/1)
- To zadanie: **[Fala 0] Wypełnić manifest chain/DAO** — [#2](https://github.com/ipicoin/universal-independency-declaration/issues/2)

## Spis treści

1. [Statement niezależności](#1-statement-niezależności)
2. [Parametry genesis (DRAFT)](#2-parametry-genesis-draft)
3. [Tokenomika (DRAFT)](#3-tokenomika-draft)
4. [Model federacji i zarządzania DAO](#4-model-federacji-i-zarządzania-dao)
5. [Powiązane repozytoria](#5-powiązane-repozytoria)
6. [Status i proces akceptacji](#6-status-i-proces-akceptacji)

Pełne parametry techniczne w postaci zwięzłej tabeli referencyjnej znajdują się
w pliku [`MANIFEST.md`](./MANIFEST.md).

---

## 1. Statement niezależności

My, uczestnicy **Independent Protocol Infrastructure**, deklarujemy, że:

- **Suwerenność organizacji.** Każda organizacja przystępująca do federacji
  zachowuje pełną autonomię wewnętrzną — swoje procedury, tożsamość i granice
  przywilejów. IPI nie jest strukturą nadrzędną, lecz *nośnikiem konsensusu*
  (consensus stabilisation carrier) pomiędzy równorzędnymi podmiotami.
- **Wspólne, respektowane granice.** Federacja opiera się na dobrowolnym
  współdzieleniu zasobów przy jednoczesnym poszanowaniu granic przywilejów
  każdego uczestnika. Współpraca nie znosi niezależności — utrwala ją.
- **Protokół ponad pośrednika.** Zaufanie budujemy na weryfikowalnym protokole i
  otwartym kodzie, a nie na zaufanej stronie trzeciej. Kluczowe decyzje sieci
  podejmowane są on-chain, transparentnie i odwracalnie wyłącznie przez kolejną
  decyzję DAO.
- **Wolność jednostki, niezależność grup.** Narzędzia IPI mają czynić jednostki
  wolnymi, a grupy niezależnymi — przy współdzielonych i wzajemnie
  respektowanych granicach uprawnień.

Ta deklaracja jest częścią manifestu i stanowi jego warstwę aksjologiczną:
parametry techniczne poniżej są jedynie jej implementacją.

---

## 2. Parametry genesis (DRAFT)

> Wartości poniżej to **propozycja robocza** do przeglądu i decyzji DAO.

| Parametr | Wartość (propozycja) | Uwagi |
|---|---|---|
| Chain ID (mainnet) | `ipi-mainnet-2` | inkrementowany przy upgrade wymagającym hard-forku |
| Chain ID (testnet) | `ipi-testnet-1` | publiczny devnet/testnet — Fala 1 |
| Bech32 prefix (account) | `ipi` | walidator: `ipivaloper`, konsensus: `ipivalcons` |
| SLIP-0044 coin type | `118` | wspólny dla ekosystemu Cosmos (interop, Ledger, walletty) |
| RPC / REST endpoint | `https://ipicoin.eu/rpc` / `https://ipicoin.eu/api` | wg `chainconfig` (SSOT) |
| Denom bazowy | `nipi` | jednostka bazowa on-chain (exponent 0); 1 IPI = 1 000 000 000 nipi |
| Denom display | `ipi` | jednostka display on-chain (lowercase), `exponent = 9` |
| Symbol / ticker | `IPI` | kanoniczny symbol (uppercase) wyświetlany użytkownikowi |
| Docelowy czas bloku | ~6 s | Tendermint/CometBFT BFT |
| Silnik konsensusu | CometBFT (Tendermint BFT) | via Cosmos SDK |
| Środowisko kontraktów | CosmWasm (WASM) | zgodnie z `cw-template`, `minimal-gas-meter` |
| Początkowy zestaw walidatorów | do ustalenia (genesis validators) | rekrutacja w Fali 1 (devnet/testnet) |

Docelowa migracja node'a: `ipid` → customized `wasmd`
([`independency-daemon`](https://github.com/ipicoin/independency-daemon), Fala 1).
Jedno źródło prawdy dla parametrów sieci będzie utrzymywane w repo
[`chainconfig`](https://github.com/ipicoin/chainconfig) — niniejszy manifest
opisuje intencję, `chainconfig` dostarcza maszynowo-czytelnej konfiguracji.

---

## 3. Tokenomika (DRAFT)

> **Cała tokenomika poniżej to PROPOZYCJA do decyzji DAO.** Konkretne liczby
> (supply, procenty alokacji, stopy inflacji) wymagają osobnej uchwały
> governance i mogą ulec zmianie przed genesis.

**Token natywny:** symbol **IPI** (uppercase, kanon). Jednostka bazowa on-chain
`nipi` (exponent 0), jednostka display on-chain `ipi` (exponent 9). Pełni trzy
funkcje: opłaty (gas), staking/zabezpieczenie konsensusu oraz prawo głosu w
governance.

### 3.1 Podaż początkowa (propozycja)

- **Initial supply:** propozycja **1 000 000 000 IPI** (`1e18 nipi`) w bloku
  genesis. Wartość do zatwierdzenia przez DAO.
- Podaż nie jest sztywno ograniczona — rośnie zgodnie z modelem inflacji
  stakingowej (poniżej), z jednoczesnym spalaniem części opłat (do decyzji DAO).

### 3.2 Model inflacji (propozycja, wzorzec `x/mint` Cosmos SDK)

| Parametr | Wartość (propozycja) | Opis |
|---|---|---|
| Inflacja min. | 7% rocznie | dolna granica emisji |
| Inflacja maks. | 20% rocznie | górna granica emisji |
| Cel bonded (goal bonded) | 67% | docelowy udział stakowanych tokenów |
| Tempo zmiany inflacji | 13% / rok | prędkość dochodzenia do celu |

Mechanizm jest adaptacyjny: gdy udział stakowanych tokenów spada poniżej celu,
inflacja rośnie (zachęta do stakowania i wzmocnienia bezpieczeństwa); gdy
przekracza cel — maleje.

### 3.3 Community pool i opłaty (propozycja)

- **Community tax:** propozycja **2%** nagród kierowanych do community pool
  (finansowanie rozwoju federacji przez governance).
- **Opłaty (gas):** płacone w `nipi`; część może być spalana (deflacyjny
  bufor) — udział do decyzji DAO.

### 3.4 Dystrybucja podaży początkowej (PROPOZYCJA — do decyzji DAO)

> **Nagrody stakingowe pochodzą wyłącznie z inflacji** (moduł `x/mint`, §3.2):
> `x/mint` mintuje **nowe** tokeny ponad bieżącą podaż i kieruje je do puli
> nagród walidatorów/delegatorów w każdym bloku. Dlatego **nie ma osobnej
> „rezerwy stakingowej" w genesis** — utrzymywanie 20% podaży początkowej jako
> rezerwy nagród oznaczałoby **podwójne zaprowiantowanie** (rewards raz z
> genesis, raz z emisji inflacyjnej). Zwolnione 20% realokujemy do pozostałych
> pul; nowy podział sumuje się do 100%. **To propozycja DRAFT do decyzji DAO.**

| Pula | Udział (propozycja) | Vesting / uwagi |
|---|---|---|
| Federacja / organizacje członkowskie | 35% | dystrybucja wg wejścia do federacji |
| Community pool / skarb DAO | 30% | zarządzane on-chain przez governance |
| Zespół rdzeniowy i wcześni kontrybutorzy | 18% | vesting liniowy, np. 4 lata / 1 rok cliff |
| Ekosystem, granty, incentywy | 17% | programy rozwojowe, faucet testnet |

**Suma: 100%** (35 + 30 + 18 + 17). Bezpieczeństwo stakingu finansuje wyłącznie
emisja inflacyjna z §3.2 — nie genesis.

> Powyższe **procenty są jedynie propozycją wyjściową**. Ostateczna alokacja,
> harmonogramy vestingu i ewentualny hard cap wymagają uchwały DAO przed genesis.

---

## 4. Model federacji i zarządzania DAO

### 4.1 Jak organizacje dołączają

1. **Zgłoszenie (proposal on-chain).** Organizacja składa propozycję
   przystąpienia do federacji przez governance IPI DAO (aplikacja
   [`Ivote`](https://github.com/ipicoin/Ivote)).
2. **Tożsamość i uwierzytelnienie.** Organizacja rejestruje swoją tożsamość
   zgodnie z architekturą auth IPI (repo
   [`.github`](https://github.com/ipicoin/.github), Fala 0): warstwa sprzętowa
   **WebAuthn + YubiKey (PIV) + NTAG424 DNA**, z możliwością mapowania na
   **katalog organizacyjny (LDAP)** i **zdecentralizowane identyfikatory (DID)**
   dla podmiotów i ich reprezentantów.
3. **Głosowanie.** Członkowie DAO głosują nad przyjęciem (staking-weighted).
4. **Onboarding operacyjny.** Po akceptacji organizacja podłącza swoje usługi do
   wspólnego środowiska orkiestracji
   [`hq-spacecraft`](https://github.com/ipicoin/hq-spacecraft) — standardowej,
   otwartej bazy usług dla „rozproszonych, federowanych organizacji
   zjednoczonych zaangażowaniem w IPI", gdzie **IPI działa jako nośnik
   stabilizacji konsensusu** pomiędzy różnorodnymi podmiotami.

### 4.2 Rola LDAP / DID

- **DID** — warstwa międzyorganizacyjna: każdy podmiot i kluczowy aktor
  federacji ma weryfikowalny, przenośny identyfikator niezależny od pojedynczego
  dostawcy. DID wiąże tożsamość z kluczami sprzętowymi (YubiKey/NTAG424) i z
  adresami on-chain (`ipi…`).
- **LDAP** — warstwa wewnątrzorganizacyjna: organizacje mogą mapować własne
  katalogi (role, zespoły) na role w federacji, zachowując autonomię zarządzania
  dostępem u siebie. LDAP pozostaje po stronie organizacji; do federacji
  eksportowane są wyłącznie potrzebne, poświadczone atrybuty.

### 4.3 IPI jako nośnik konsensusu (carrier)

IPI nie centralizuje władzy — dostarcza **wspólną warstwę konsensusu i
rozliczalności**, na której niezależne organizacje mogą się porozumiewać bez
zaufanej strony trzeciej. Łańcuch jest „carrierem": stabilizuje uzgodnienia
(płatności, głosowania, zapisy tożsamości, NFT/atesty) między różnorodnymi
podmiotami, pozostawiając im wewnętrzną suwerenność.

### 4.4 Proces governance

- **Propozycje:** składane on-chain, z okresem depozytu i okresem głosowania.
- **Głosowanie:** ważone stakiem (`Yes` / `No` / `NoWithVeto` / `Abstain`),
  wzorzec modułu `x/gov` Cosmos SDK.
- **Egzekucja — dwa typy propozycji (`x/gov`):**
  - *Propozycje wykonywalne* (z executable messages) — np. zmiana parametrów
    (`MsgUpdateParams`), wydatek z community pool (`MsgCommunityPoolSpend`),
    software upgrade: po przyjęciu **wykonują się automatycznie on-chain** (na
    koniec okresu głosowania).
  - *Propozycje tekstowe / sygnalizacyjne* — np. **przyjęcie organizacji do
    federacji**: `x/gov` **nie przewiduje dla nich on-chain execution**.
    Przegłosowanie jest wiążącym sygnałem DAO, a właściwe wdrożenie następuje
    **off-chain / manualnie** (onboarding operacyjny wg §4.1: rejestracja
    tożsamości, podłączenie do `hq-spacecraft`).
- **Odwracalność:** każda decyzja może zostać zmieniona wyłącznie kolejną
  decyzją DAO — brak uprzywilejowanego pojedynczego administratora.

---

## 5. Powiązane repozytoria

| Repo | Rola w federacji |
|---|---|
| [`independency-daemon`](https://github.com/ipicoin/independency-daemon) | node (`ipid` → `wasmd`), ścieżka krytyczna, Fala 1 |
| [`chainconfig`](https://github.com/ipicoin/chainconfig) | jedno źródło prawdy parametrów sieci (Fala 0) |
| [`.github`](https://github.com/ipicoin/.github) | architektura tożsamości/auth (WebAuthn+YubiKey+NTAG424) |
| [`hq-spacecraft`](https://github.com/ipicoin/hq-spacecraft) | orkiestracja usług federacji, IPI jako carrier konsensusu |
| [`standard_repo_template`](https://github.com/ipicoin/standard_repo_template) | standard repo (B.O.S./GRIH/LICENSE/CONTRIBUTING) |
| [`wallet-core.js`](https://github.com/ipicoin/wallet-core.js) | rdzeń portfela / podpisywanie |
| [`Ivote`](https://github.com/ipicoin/Ivote) · [`Iswap`](https://github.com/ipicoin/Iswap) · [`ipi-nft`](https://github.com/ipicoin/ipi-nft) · [`ipi-rpc`](https://github.com/ipicoin/ipi-rpc) | aplikacje DAO |

---

## 6. Status i proces akceptacji

Ten dokument ma status **DRAFT**. Ścieżka do wejścia w życie:

1. Przegląd społeczności i zespołu rdzeniowego (PR / dyskusja w issue #2).
2. Doprecyzowanie liczb tokenomiki i alokacji (osobna propozycja governance).
3. Synchronizacja parametrów genesis z repo `chainconfig` (jedno źródło prawdy).
4. Uchwała DAO zatwierdzająca manifest → zdjęcie statusu DRAFT.

Realizuje **Fala 0** roadmapy IPI ([#1](https://github.com/ipicoin/universal-independency-declaration/issues/1)).
Zamyka **[#2](https://github.com/ipicoin/universal-independency-declaration/issues/2)**.
