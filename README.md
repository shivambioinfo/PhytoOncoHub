# PhytoOncoHub

**Phytochemical screening for cancer protein targets**

Open-source pre-screen that ranks plant compounds against oncology proteins (EGFR, HER2, VEGFR2, PI3K, AKT, mTOR, BRAF, CDK6, BCL-2, Topo IIα, HDAC2, ERα, COX-2, tubulin), flags simple drug-likeness / alert issues, and writes a compound–target–pathway table.

> Research aid only. This is not a diagnostic or treatment tool. Scores are heuristics, not docking ΔG and not measured IC50.

## GitHub About text

```text
Open toolkit for ranking phytochemicals against cancer protein targets, with drug-likeness flags and compound–target–pathway mapping. Research pre-screen only.
```

Topics: `phytochemicals` `cancer` `network-pharmacology` `admet` `bioinformatics` `natural-products` `drug-discovery`

## Install

```bash
cd PhytoOncoHub
python -m venv .venv
source .venv/bin/activate
pip install -e .
```

Optional: `pip install rdkit`

## Commands

```bash
python -m phytooncohub catalog --kind targets
python -m phytooncohub catalog --kind phytos

python -m phytooncohub screen --target egfr --out reports/egfr.md
python -m phytooncohub screen --target vegfr2 --out reports/vegfr2.md

python -m phytooncohub network --top-per-compound 3 --out reports/network.md

python -m phytooncohub score \
  --name quercetin \
  --smiles "C1=CC(=C(C=C1C2=C(C(=O)C3=C(C=C(C=C3O2)O)O)O)O)O" \
  --class-name flavonoid \
  --target pi3k
```

## Where to add data

| Add this | File |
| --- | --- |
| New phytochemical | `data/phytochemicals.csv` |
| New cancer protein | `data/targets.json` |
| New pathway grouping | `data/pathways.json` |

CSV columns:

```text
id,name,class,plant,smiles,cancer_prior,reported_targets,notes
```

`cancer_prior` = `high`, `medium`, or `low`.

## Scoring

```
rank = 0.28 * cancer_prior
     + 0.32 * target_fit
     + 0.25 * druglikeness
     + 0.15 * alert_score
```

Drug-likeness uses simple Lipinski/Veber-style cuts (MW, logP, HBD/HBA, rotors, TPSA).

## Limitations

- No AutoDock / AlphaFold job is run.
- The compound table is a starter set, not a complete natural-product database.
- Catechol and polyphenol alerts are developability flags, not proof of toxicity.
- Paclitaxel is included only as a reference natural product.

## License

MIT.
