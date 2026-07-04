# IPI Manifest — parametry referencyjne

> **DRAFT — wymaga akceptacji DAO.** Zwięzła, maszynowo-czytelna tabela
> parametrów sieci IPI. Opis narracyjny i uzasadnienia znajdują się w
> [`README.md`](./README.md). W razie rozbieżności obowiązuje decyzja DAO oraz
> repo [`chainconfig`](https://github.com/ipicoin/chainconfig) jako jedno źródło
> prawdy dla wartości maszynowych.

## Tożsamość sieci

| Klucz | Wartość (propozycja) |
|---|---|
| `network_name` | Independent Protocol Infrastructure (IPI) |
| `chain_id_mainnet` | `ipi-mainnet-2` |
| `chain_id_testnet` | `ipi-testnet-1` |
| `consensus` | CometBFT (Tendermint BFT) |
| `sdk` | Cosmos SDK |
| `smart_contracts` | CosmWasm (WASM) |
| `target_block_time` | `~6s` |
| `rpc_endpoint` | `https://ipicoin.eu/rpc` |
| `rest_endpoint` | `https://ipicoin.eu/api` |

## Adresacja i denominacja

| Klucz | Wartość (propozycja) |
|---|---|
| `bech32_prefix_account` | `ipi` |
| `bech32_prefix_validator` | `ipivaloper` |
| `bech32_prefix_consensus` | `ipivalcons` |
| `slip44_coin_type` | `118` |
| `base_denom` | `nipi` (jednostka bazowa on-chain, exponent 0) |
| `display_denom` | `ipi` (jednostka display on-chain, lowercase, exponent 9) |
| `symbol` / `ticker` | `IPI` (kanon, uppercase — symbol wyświetlany użytkownikowi) |
| `denom_exponent` | `9` |
| `conversion` | `1 IPI = 1_000_000_000 nipi` |

## Tokenomika (propozycja — decyzja DAO wymagana)

| Klucz | Wartość (propozycja) |
|---|---|
| `initial_supply` | `1_000_000_000 IPI` (`1e18 nipi`) |
| `inflation_min` | `0.07` |
| `inflation_max` | `0.20` |
| `goal_bonded` | `0.67` |
| `inflation_rate_change` | `0.13` |
| `community_tax` | `0.02` |

### Dystrybucja podaży początkowej (propozycja — do decyzji DAO)

> **DRAFT.** Nagrody stakingowe pochodzą **wyłącznie z inflacji** (`x/mint`) —
> brak osobnej puli „rezerwy stakingowej" w genesis (unika podwójnego
> zaprowiantowania). Zwolnione 20% realokowane do pozostałych pul; suma = 100%.

| Pula | Udział |
|---|---|
| Federacja / organizacje członkowskie | 35% |
| Community pool / skarb DAO | 30% |
| Zespół rdzeniowy i wcześni kontrybutorzy | 18% |
| Ekosystem / granty / incentywy | 17% |

## Governance

| Klucz | Wartość (propozycja / wzorzec) |
|---|---|
| `gov_module` | Cosmos SDK `x/gov` |
| `vote_options` | `Yes` / `No` / `NoWithVeto` / `Abstain` |
| `voting_power` | staking-weighted |
| `execution_executable` | propozycje z executable messages (zmiana parametrów, wydatek z community pool, upgrade) — **auto-exec on-chain** po przyjęciu |
| `execution_text` | propozycje tekstowe/sygnalizacyjne (np. przyjęcie organizacji) — **brak on-chain execution**, wdrożenie off-chain/manualne |
| `reversibility` | wyłącznie kolejną decyzją DAO |

## Tożsamość i federacja

| Klucz | Wartość (propozycja) |
|---|---|
| `auth_hardware` | WebAuthn + YubiKey (PIV) + NTAG424 DNA |
| `inter_org_identity` | DID |
| `intra_org_identity` | LDAP (po stronie organizacji) |
| `consensus_role` | IPI jako carrier stabilizacji konsensusu |
| `orchestration` | `hq-spacecraft` |
| `auth_spec_repo` | `.github` |

---

Status: **DRAFT**. Realizuje Fala 0
([#1](https://github.com/ipicoin/universal-independency-declaration/issues/1)).
Zamyka [#2](https://github.com/ipicoin/universal-independency-declaration/issues/2).
