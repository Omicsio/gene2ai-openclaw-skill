# Gene2AI OpenClaw Skill

Your personal health data hub for AI agents. Query genomic insights (health risks, drug responses, CYP450 metabolizer status, HLA allele typing, APOE genotype, nutrition, traits, ancestry), upload medical documents for AI-powered parsing, record daily health metrics, and explore genomic-lab cross-references — all through natural conversation with your AI agent.

## What's New in v3.0

- **Specialized Genomic Categories**: CYP450 metabolizer phenotyping, HLA allele typing, APOE genotyping — each with structured, parsed data
- **Subcategory Filtering**: Query specific genomic categories (e.g., `?subcategory=cyp450`) instead of fetching everything
- **Grouped Format**: Get records organized by category → subcategory hierarchy with `?format=grouped`
- **Risk Overview API**: Comprehensive dashboard combining elevated genomic risks + abnormal lab values + cross-references
- **Genomic-Lab Cross-References**: Bidirectional links between lab indicators and genomic markers (e.g., LDL-C ↔ cholesterol genes)
- **Enriched Genomic Records**: `data` field auto-parses valueText JSON — no manual parsing needed
- **Crawler-Friendly Docs**: API documentation at gene2.ai/guide is fully indexable by search engines and AI agents

## Installation

### From ClawHub

```bash
clawhub install gene2ai
```

### Manual Installation

Copy the `SKILL.md` file to your OpenClaw skills directory:

```bash
mkdir -p ~/.openclaw/workspace/skills/gene2ai
cp SKILL.md ~/.openclaw/workspace/skills/gene2ai/
```

Then ask your agent to "refresh skills" or restart the OpenClaw gateway.

## Setup

### Step 1: Create a Gene2AI Account

1. Visit [gene2.ai](https://gene2.ai) and create an account
2. (Optional) Upload your raw genetic data file (23andMe, AncestryDNA, or WeGene) for genomic analysis
3. (Optional) Upload lab reports or checkup results for AI-powered parsing

### Step 2: Generate an API Key

1. Go to [gene2.ai/api-keys](https://gene2.ai/api-keys)
2. Click **Generate New Key** — this creates a unified key that grants access to all your health data (genomic + medical + self-reported)
3. Copy the API key (valid for 30 days)

### Step 3: Configure in OpenClaw

Edit your `~/.openclaw/openclaw.json`:

```json
{
  "skills": {
    "entries": {
      "gene2ai": {
        "enabled": true,
        "apiKey": "your-api-key-here"
      }
    }
  }
}
```

## Usage Examples

Once configured, you can interact with your AI assistant naturally:

### Genomic Queries
- "What does my DNA say about caffeine metabolism?"
- "Am I at genetic risk for type 2 diabetes?"
- "What's my CYP2D6 metabolizer status?"
- "Do I carry the APOE ε4 allele?"
- "Am I at risk for abacavir hypersensitivity? (HLA-B*57:01)"
- "What vitamins should I supplement based on my genetics?"
- "Tell me about my ancestry composition"

### Cross-Reference Queries
- "My LDL cholesterol is high — do I have related genetic risk factors?"
- "Give me a comprehensive health risk overview"
- "What genomic markers are related to my thyroid function?"

### Health Document Upload
- Send a photo of your blood test report → Agent uploads and reports extracted indicators
- Send a PDF of your annual checkup → Agent parses all results and highlights abnormalities
- "帮我保存这个体检报告" → Agent confirms and uploads

### Daily Health Metrics
- "My blood pressure is 125/82, heart rate 72"
- "血糖 5.8, 体重 72kg"
- "Record my temperature: 36.8°C"

### Health Data Queries
- "What's in my health vault?"
- "Show me my recent lab results"
- "给我看看我的健康数据概况"

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/v1/health-data/summary` | GET | Health data overview with category breakdown |
| `/api/v1/health-data/full` | GET | All records (supports `?category`, `?subcategory`, `?format=grouped`) |
| `/api/v1/health-data/delta` | GET | Incremental changes since a version |
| `/api/v1/health-data/risk-overview` | GET | **NEW** Comprehensive risk dashboard (genomic + lab + cross-refs) |
| `/api/v1/health-data/genomic-links/:code` | GET | **NEW** Genomic markers related to a lab indicator |
| `/api/v1/health-data/lab-genomic-summary` | GET | **NEW** All lab indicators with genomic cross-reference counts |
| `/api/v1/health-data/upload` | POST | Upload document for AI parsing |
| `/api/v1/health-data/doc/:id` | GET | Query document parse status |
| `/api/v1/health-data/records` | POST | Submit structured health records |
| `/api/v1/genomics/:jobId` | GET | Legacy: query genomic data by job ID |

Full API documentation: [gene2.ai/guide](https://gene2.ai/guide)

## Genomic Subcategories

| Subcategory | Count | Description |
|-------------|-------|-------------|
| `health_risk` | 193 | Disease risk assessments (CFTR, LDLR, Alzheimer's, CAD, T2D, etc.) |
| `drug_response` | 61 | Pharmacogenomic predictions (Warfarin, Clopidogrel, SSRIs, Codeine) |
| `nutrition` | 32 | Nutrigenomics (Vitamin D, Folate/MTHFR, B12, Omega-3, Iron) |
| `trait` | 21 | Genetic traits (hair color, muscle type, caffeine, lactose) |
| `hla` | 9 | HLA allele typing (drug hypersensitivity, autoimmune associations) |
| `ancestry` | 4 | Regional ancestry percentages |
| `cyp450` | 3 | CYP450 metabolizer phenotyping (CYP2D6, CYP2C19, CYP2C9) |
| `apoe` | 1 | APOE genotype (ε2/ε3/ε4, Alzheimer's & cardiovascular risk) |

## Supported Lab Indicator Cross-References

The following lab indicator codes can be used with `/genomic-links/:code`:

| Code | Name | Related Genomic Conditions |
|------|------|---------------------------|
| TC, TG, LDL-C, HDL-C | Lipid Panel | Familial Hypercholesterolemia, CAD |
| SBP, DBP | Blood Pressure | Hypertension genes |
| FBG, HbA1c | Blood Glucose | Type 2 Diabetes genes |
| BMI | Body Mass Index | Obesity-related variants |
| UA | Uric Acid | Gout-related genes |
| TSH, FT3, FT4 | Thyroid Function | Thyroid disease genes |
| ALT, AST, GGT, ALP, TBIL | Liver Function | Liver disease genes |
| SCr, BUN | Kidney Function | Kidney disease genes |
| WBC, HGB, PLT | Blood Count | Hematological conditions |
| CRP | Inflammation | Inflammatory disease genes |

## Supported Document Types

- PDF reports (lab results, checkup summaries, medical records)
- Photos of printed reports (JPEG, PNG, HEIC)
- Max file size: 50MB
- Languages: English, Chinese (auto-detected)

## Security

- All data transmitted over HTTPS
- Bearer token authentication (JWT)
- API keys expire after 30 days (regenerate at gene2.ai/api-keys)
- Keys can be revoked at any time
- No health data is cached locally by the agent

## Links

- **Website**: [gene2.ai](https://gene2.ai)
- **Health Data Vault**: [gene2.ai/health-data](https://gene2.ai/health-data)
- **Dashboard**: [gene2.ai/dashboard](https://gene2.ai/dashboard)
- **API Documentation**: [gene2.ai/guide](https://gene2.ai/guide)
- **API Key Management**: [gene2.ai/api-keys](https://gene2.ai/api-keys)

## Changelog

### v3.0.0
- Added specialized genomic subcategories: CYP450, HLA, APOE with structured parsed data
- Added subcategory filtering (`?subcategory=cyp450`) and grouped format (`?format=grouped`)
- Added risk-overview endpoint combining genomic risks + lab abnormalities + cross-references
- Added genomic-links endpoint for lab indicator ↔ genomic marker cross-references
- Added lab-genomic-summary endpoint for cross-reference dashboard
- Enriched genomic records with auto-parsed `data` field
- Added SEO prerender for crawler-friendly API documentation

### v2.0.0
- Added health document upload with AI-powered parsing
- Added structured health metrics submission
- Added health data query endpoints (summary, full, delta)
- Unified genomic and health data access under single skill
- Added source tracking for multi-channel data collection

### v1.0.0
- Initial release with genomic data query support
- 5 categories: health risks, drug responses, traits, nutrition, ancestry
