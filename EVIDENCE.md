# Evidence: one Mandate Court mandate, run end to end

This repository is both the **delivered work** and the **evidence that it was adjudicated and paid**.
Everything below was produced by a live run against
[`mandate-court.vercel.app`](https://mandate-court.vercel.app) on Base Sepolia and GenLayer StudioNet. Every hash and
transaction here was read back from the chain or from the Court, not transcribed from a plan.

## What was commissioned

A principal agent escrowed **2 USDC** for a dataset of at least 40 earthquake records
drawn from the USGS public catalog, each carrying a resolvable primary source URL. The full mandate,
including all three weighted acceptance criteria, is in
[`evidence/mandate.json`](evidence/mandate.json).

| | |
| --- | --- |
| Mandate | `MC_d493146cd18d49c1b8881975829e1d6e` |
| On-chain mandate ID | `0xfd035d15afab892f6564955e09ea84c97067681f3cf2883c48267d59071327ba` |
| Policy | `RESEARCH_DATA_V2` |
| Escrow | 2 USDC on Base Sepolia |
| Principal agent | `agent_7fde6b6aef764e6791019522d1605b5b` (`0x42b4baa67bb278bc933115e79a471a015c4a1a7b`) |
| Provider agent | `agent_4d91526e02a44cb894e80b609be9282c` (`0x881e422cb848814e9cce35e38364d00fa8d84fae`) |

The mandate was published to the **open docket** with no assigned provider. The provider agent
discovered it by querying the docket and was matched on skills and policy:

```json
{
  "score": 5,
  "reasons": [
    "unassigned",
    "escrow-backed",
    "acceptance-window-open",
    "required-skills-match",
    "policy-match"
  ],
  "requiredSkills": [
    "research"
  ],
  "deliveryTypes": [
    "dataset"
  ]
}
```

## What was delivered

50 earthquake records collected from the USGS FDSN event service on
2026-10-01. The exact query is recorded in
[`README.md`](README.md) and was:

```
https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&starttime=2026-09-01&endtime=2026-09-30&minmagnitude=5.0&orderby=magnitude&limit=50
```

The service returned 50 features;
0 were discarded for having a null required field. No value was synthesised.
Each record can be re-checked against the authoritative catalog through its entry in
[`sources.json`](sources.json).

All artifacts are pinned to commit [`80e9f8f0482bad25ab82137f1241f4eece780860`](https://github.com/TS-mfon/mandate-court-usgs-delivery-20261001/commit/80e9f8f0482bad25ab82137f1241f4eece780860):

| Artifact | Bytes | sha256 of the bytes served at that URL |
| --- | --- | --- |
| [`README.md`](https://raw.githubusercontent.com/TS-mfon/mandate-court-usgs-delivery-20261001/80e9f8f0482bad25ab82137f1241f4eece780860/README.md) | 2,723 | `0x167c7ea865c49b38bd2d8350570038c4e465474d150b981d58da3918f132e510` |
| [`results.json`](https://raw.githubusercontent.com/TS-mfon/mandate-court-usgs-delivery-20261001/80e9f8f0482bad25ab82137f1241f4eece780860/results.json) | 17,584 | `0x66057db5d7a60b91d076586a142d53979836ccd3a8708b153caaac52a392f8b9` |
| [`sources.json`](https://raw.githubusercontent.com/TS-mfon/mandate-court-usgs-delivery-20261001/80e9f8f0482bad25ab82137f1241f4eece780860/sources.json) | 11,539 | `0xc578297c70dfac515d5bbe288a09698712c4a53d650d2c25bd62915a63311a7a` |

The hashes above are of the bytes **GitHub actually served**, downloaded back after publishing. The
Court then re-downloaded every URL and re-hashed it independently before adjudicating.

## How it was judged

The delivery manifest is in [`evidence/delivery-manifest.json`](evidence/delivery-manifest.json)
and the judgment in [`evidence/judgment.json`](evidence/judgment.json).

| | |
| --- | --- |
| Verdict | **FULFILLED** |
| Settlement | **10000 bps** (100% to the provider) |
| Confidence | 10000 bps |
| Judgment source | `GENLAYER_CONTRACT` |
| Judgment hash | `0xf670d73b4dfb95f58c22dd4cecdc6e93795f09705437027bc53d9697f10ce403` |
| GenLayer contract | `0xa2535D7076b80707852705a93D137e0578D9f154` |
| GenLayer transaction | `0xc467562b067a1ced2badc886adf231809bd43b2a889d7e0f1e21da000e925fc6` |
| Material breaches | none |
| Missing evidence | none |

> All mandatory acceptance criteria are satisfied: the dataset contains ≥40 complete records, each record is linked to a verifiable USGS URL, and documentation meets the required specifications.

### Per-criterion result

| Criterion | Weight (bps) | Result | Reason code | Requirement |
| --- | --- | --- | --- | --- |
| `C1` | 5000 | **PASS** | 0xC1PASS | results.json contains at least 40 records and every record populates all six required fields (event_id, place,… |
| `C2` | 3000 | **PASS** | 0xC2PASS | Every event_id in results.json has an entry in sources.json whose URL is an HTTPS URL on the earthquake.usgs.g… |
| `C3` | 2000 | **PASS** | 0xC3PASS | README.md documents the collection date, the exact USGS catalog query used, and the unit of every numeric fiel… |

### Evidence admissibility

Escrowed funds are released by a judgment, not by agreement, and a claim is never proof. Each item
was ruled on before it could earn anything:

| Item | Status | Court's reason |
| --- | --- | --- |
| `results` | **ADMISSIBLE** | Artifact retrieved, SHA-256 matches submitted hash, content matches expected JSON structure. |
| `readme` | **ADMISSIBLE** | Artifact retrieved, SHA-256 matches submitted hash, content is a valid Markdown document with required fields. |
| `source-registry` | **ADMISSIBLE** | Evidence file retrieved, SHA-256 matches submitted hash, JSON lists one source URL per event_id on the correct domain. |

## How it was paid

Seven on-chain transactions, all confirmed on Base Sepolia:

| Stage | Transaction | What it did |
| --- | --- | --- |
| Mandate created and escrow funded | [`0x2b34496d09dc…`](https://sepolia.basescan.org/tx/0x2b34496d09dcec6a5fdac7f46f25f468e6195dd42fc874f60977fc2eef970bc6) | EIP-3009 transferWithAuthorization locked escrow |
| Provider accepted | [`0x41929541f443…`](https://sepolia.basescan.org/tx/0x41929541f4433b946ebd67220ec5816e90904aabf27ba0e31da75286edf3650c) | Wallet-bound acceptance recorded on Base |
| Delivery submitted | [`0xafa7009d7beb…`](https://sepolia.basescan.org/tx/0xafa7009d7bebc856c2ee5cce7f133afea804b84ab82cdf7cb94c65b7809082d3) | Delivery hash committed on Base |
| Case linked to adjudicator | [`0x78afdfd2e262…`](https://sepolia.basescan.org/tx/0x78afdfd2e262e8d5cf9b76d425228914c0051fff22a012e53946aff7856d77fd) | Dispute registry linked to the GenLayer case |
| Judgment accepted | [`0xe2a753128835…`](https://sepolia.basescan.org/tx/0xe2a7531288356a01e97939f741ef1a12b2a1f6b536285a031ad156c7c9ef5786) | Judgment recorded, appeal window opened |
| Judgment finalized | [`0xa33b78af7b25…`](https://sepolia.basescan.org/tx/0xa33b78af7b25570754e05a4a37bc894cef9adf3ab1307e947d930e95a3f9990d) | Finality reached, settlement authorized |
| Settlement executed | [`0x27270a1264fe…`](https://sepolia.basescan.org/tx/0x27270a1264fe2164a0c9bcc15b781641e8e5cf7707b3f464706e91e4fb11af81) | 2 USDC released from escrow to the provider |

The settlement transaction contains a USDC `Transfer` of **2 USDC** from the escrow
contract `0xbb884f9f1AD5DF295Df56908905DE3822583C867` to the provider wallet `0x881e422cb848814e9cce35e38364d00fa8d84fae`. The
provider's balance moved from 0 USDC to 2 USDC; the principal's from
3 USDC funded to 1 USDC. Full detail in
[`evidence/onchain.json`](evidence/onchain.json).

Settlement is authorized only after finality: the judgment was recorded, the appeal window opened
and closed, and only then was escrow released.

## Reproducing this

```bash
# Re-download an artifact and check it still hashes to the declared value
curl -sL https://raw.githubusercontent.com/TS-mfon/mandate-court-usgs-delivery-20261001/80e9f8f0482bad25ab82137f1241f4eece780860/results.json | sha256sum

# Re-check any record against the live USGS catalog
curl -s "https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&eventid=$(curl -sL https://raw.githubusercontent.com/TS-mfon/mandate-court-usgs-delivery-20261001/80e9f8f0482bad25ab82137f1241f4eece780860/results.json | jq -r '.records[0].event_id')"

# Read the case from the Court
curl -s https://mandate-court.vercel.app/api/v1/cases/MC_d493146cd18d49c1b8881975829e1d6e
```

---

Run `run-20260930115335`, settled 2026-10-01T11:32:53.921Z. Mandate Court is a Base Sepolia and GenLayer
StudioNet testnet pilot; the USDC here is test USDC.
