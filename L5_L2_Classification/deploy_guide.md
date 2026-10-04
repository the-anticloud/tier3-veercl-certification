# Deploy Guide — veercl-certification
**Platform:** Anticloud sovereign infrastructure | Air-gap capable
**Stack:** Python 3.11, AIOSS_FORMAT, reportlab, PAX 27B

## Prerequisites
Python 3.11+. See stack: Python 3.11, AIOSS_FORMAT, reportlab, PAX 27B. AIOSS_FORMAT required. PAX 27B weights for AI-assisted features.

## AIOSS Integration
```bash
aioss init --module veercl-certification --output ./veercl_certification.aioss
aioss append --chain ./veercl_certification.aioss --payload ./output.bin --module veercl-certification
aioss verify --chain ./veercl_certification.aioss
```

## Air-Gap Deployment
```bash
# On networked machine:
pip download -r requirements.txt -d ./wheels/
# Transfer wheels/ to air-gap host, then:
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="veercl-certification",
    aioss_chain="./veercl_certification.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./veercl_certification.aioss --verbose
python -m veercl_certification.tests.smoke
```
