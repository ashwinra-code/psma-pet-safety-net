# PSMA-PET Incidental Finding Defender

A clinical decision support tool using a 3-agent AI pipeline for systematic detection and categorization of incidental findings in PSMA-PET scans for prostate cancer patients.

---

## ⚠️ IMPORTANT DISCLAIMER

**FOR RESEARCH AND EDUCATIONAL PURPOSES ONLY**

This tool is:
- ❌ **NOT FDA-approved or cleared for clinical use**
- ❌ **NOT intended for clinical diagnosis or treatment decisions**
- ❌ **NOT a substitute for professional medical judgment**
- ✅ **FOR RESEARCH AND EDUCATIONAL USE ONLY**

**Clinical Use Requirements:**
- Must be used only by qualified medical professionals
- All findings must be reviewed and verified by licensed physicians
- Final clinical decisions remain the sole responsibility of the treating physician
- This tool provides decision **support**, not decision **making**

**No Warranty:** This software is provided "as is" without warranty of any kind. The authors assume no liability for any clinical outcomes.

---

## 📋 Overview

PSMA-PET scans are highly specific for prostate cancer but can reveal clinically significant incidental findings that require follow-up. This tool systematically reviews radiology reports and applies evidence-based clinical guidelines to ensure no actionable findings are missed.

### Architecture: 3-Agent Pipeline

```
┌─────────────────┐      ┌──────────────────┐      ┌─────────────────┐
│   AGENT 1       │      │    AGENT 2       │      │    AGENT 3      │
│                 │      │                  │      │                 │
│   Findings      │─────▶│  Rules Engine    │─────▶│   Report        │
│   Extractor     │      │  Executor        │      │   Generator     │
│                 │      │                  │      │                 │
│ (Extracts all   │      │ (Matches to      │      │ (Creates        │
│  findings with  │      │  clinical rules  │      │  actionable     │
│  SUV, size)     │      │  + guidelines)   │      │  report)        │
└─────────────────┘      └──────────────────┘      └─────────────────┘
```

**Agent 1 - Findings Extractor:**
- Parses radiology report text
- Extracts: anatomical site, description, SUVmax, size, novelty status
- Filters out negative findings and physiologic uptake

**Agent 2 - Rules Engine Executor:**
- Matches findings to evidence-based clinical rules (RULES_V2)
- Categories: High Risk, Routine Surveillance, Benign/Expected
- Guidelines: Fleischner 2017, NCCN, ACR TI-RADS, ACR Adrenal Incidentaloma

**Agent 3 - Report Generator:**
- Creates structured clinical report
- Lists incidental findings requiring action
- Provides specific next steps with timelines

### Clinical Rules (RULES_V2)

Built-in evidence-based rules for:
- **Lung nodules** - Fleischner 2017 guidelines
- **Bone lesions** - NCCN Bone Metastases guidelines
- **Thyroid nodules** - ACR TI-RADS
- **Adrenal nodules** - ACR Adrenal Incidentaloma guidelines
- **Liver lesions** - ACR LI-RADS
- **Lymph nodes** - NCCN staging criteria

---

## 🚀 Quick Start

### Requirements

1. **Local LLM Server** (OpenAI-compatible API)
   - Recommended: [LM Studio](https://lmstudio.ai/), [Ollama](https://ollama.ai/), or [text-generation-webui](https://github.com/oobabooga/text-generation-webui)
   - Suggested models: Mistral 7B Instruct, Llama 2 7B, Qwen 2.5

2. **Modern Web Browser**
   - Chrome, Firefox, Safari, or Edge

### Usage

1. **Start your local LLM server**
   ```bash
   # Example with LM Studio
   # Start server on http://127.0.0.1:1234
   ```

2. **Open the tool**
   ```bash
   # Open psma_v11.html in your browser
   open psma_v11.html
   ```

3. **Configure LLM endpoint**
   - Enter your local LLM URL (default: `http://127.0.0.1:1234/v1/chat/completions`)
   - Enter model name (e.g., `mistral-7b-instruct-v0.3`)
   - Click "Test" to verify connection

4. **Analyze a report**
   - Click "Load sample #1" or "Load sample #2" to see examples
   - OR paste your own PSMA-PET report text
   - Click "Analyze"

5. **Review results**
   - **Incidental Findings**: Clinically significant findings requiring action
   - **Next Steps**: Specific recommendations with timelines
   - **Agent outputs**: View JSON from each pipeline stage (for debugging)

---

## 📁 Project Structure

```
psma-pet-safety-net/
├── README.md              # This file
├── LICENSE                # MIT License
├── CLAUDE.md              # Detailed technical documentation
├── psma_v11.html          # Main application (current version)
├── psma_v10.html          # Previous version
└── samples/
    ├── sample #1.rtf      # Sample report: lung nodule
    └── sample #2.rtf      # Sample report: thyroid + adrenal
```

---

## 🔬 Technical Details

### Technology Stack
- **Frontend**: Pure HTML + JavaScript (no dependencies)
- **LLM Integration**: OpenAI-compatible API
- **Data Flow**: Sequential 3-agent pipeline with JSON schemas
- **Validation**: Zod-like schema validation with fallback handlers

### Key Features
- ✅ Self-contained single HTML file
- ✅ Runs entirely locally (privacy-preserving)
- ✅ No external API calls (except to your local LLM)
- ✅ Sample data included for testing
- ✅ Visual progress indicators
- ✅ Dark mode debug outputs
- ✅ Copy/download results

### Versions

**v11** (Current - Recommended)
- Fixed: Sample #2 thyroid/adrenal extraction
- Added: THYROID-US rule for TI-RADS guidelines
- Added: Empty-input guard to prevent hallucinations
- Added: Negative-line filter to remove non-findings
- Expanded: Site enum (kidney, gallbladder, bladder, etc.)

**v10**
- Updated: Real sample files from RTF
- Full PSMA-PET reports with complete anatomical sections

See `CLAUDE.md` for detailed version history and architecture documentation.

---

## 📊 Sample Output

```
╔═══════════════════════════════════════════════════════════════╗
║           PSMA-PET SAFETY NET REPORT                          ║
╚═══════════════════════════════════════════════════════════════╝

┌─ INCIDENTAL FINDINGS ─────────────────────────────────────────┐

  • 4mm non-calcified pulmonary nodule in right lower lobe
    (SUV 2.1)

└───────────────────────────────────────────────────────────────┘

┌─ NEXT STEPS ──────────────────────────────────────────────────┐

  → CT chest in 3-6 months for pulmonary nodule

└───────────────────────────────────────────────────────────────┘
```

---

## 🔧 Development

### Contributing
Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Testing Checklist
- [ ] Sample #1 (lung nodule) extracts correctly
- [ ] Sample #2 (thyroid + adrenal) extracts both findings
- [ ] No hallucinations when Agent 1 fails
- [ ] Report formatting displays correctly
- [ ] Progress indicators work

### Reporting Issues
Please include:
- Browser and version
- LLM model used
- Sample report text (de-identified)
- Agent outputs (JSON from debug sections)

---

## 📚 Clinical Guidelines Referenced

- **Fleischner Society 2017** - Pulmonary nodule management
- **NCCN Guidelines** - Bone metastases, lymph node staging
- **ACR TI-RADS** - Thyroid nodule classification
- **ACR Adrenal Incidentaloma** - Adrenal nodule workup
- **ACR LI-RADS** - Liver lesion categorization

---

## 🙏 Acknowledgments

- Built with Claude Code (Anthropic)
- Inspired by clinical need for systematic incidental finding detection
- Sample reports based on de-identified clinical templates

---

## 📧 Contact

For questions, feedback, or collaboration:
- **Issues**: [GitHub Issues](https://github.com/YOUR_USERNAME/psma-pet-safety-net/issues)
- **Discussions**: [GitHub Discussions](https://github.com/YOUR_USERNAME/psma-pet-safety-net/discussions)

---

## ⚖️ License

MIT License - see [LICENSE](LICENSE) file for details.

**Disclaimer**: This license applies to the software code only. Clinical use requires appropriate institutional review, regulatory compliance, and medical oversight.

---

## 🔐 Privacy & Security

- **Local Processing**: All analysis happens on your machine
- **No External APIs**: Only communicates with your local LLM server
- **No Data Collection**: No telemetry or analytics
- **HIPAA Considerations**: De-identify all patient data before use
- **Recommendation**: Do NOT paste real patient reports without proper de-identification

---

**Last Updated**: January 2025
**Version**: v11
**Status**: Research/Educational Use Only
