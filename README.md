# Ijeoma AI

**A smart discovery assistant for West African tourism destinations.**

Ijeoma AI helps travellers, students and cultural researchers find and understand places in West Africa that fit their interests — beyond the usual hubs of Lagos, Accra and Dakar.

> Capstone project for **AI Research Foundations for Higher Education** · Nigeria Cohort 1 · Team Ijeoma AI

---

## The problem

Africa holds thousands of destinations, but outside a handful of hubs, practical information is hard to find. Places like Sukur in Nigeria or the Ruins of Loropéni in Burkina Faso are documented only thinly, and what exists is not usable: the material sits in academic papers, UNESCO dossiers and encyclopedic entries that you cannot filter by interest, skim for the gist, or ask questions of.

General-purpose AI assistants fill that gap with stereotypes and fabrications.

Sites like Osun-Osogbo in Nigeria, Djenné in Mali and Koutammakou in Togo are thoroughly recorded in UNESCO and academic archives, yet barely present in any form a traveller can act on. Tourism is also an under-utilised part of the region's economies, and West Africa is the region mainstream travel platforms serve worst.

## The approach

Ijeoma AI is a platform connecting travellers to these destinations. Its **discovery layer** answers each failure directly:

| Capability | What it does |
|---|---|
| **Classification** | Makes destinations filterable by theme, region and terrain |
| **Summarization** | Turns a dense archive passage into a short traveller digest |
| **Conversation** | Returns grounded answers to follow-up questions |

This capstone delivers the discovery layer. Guide onboarding and service integration come in a later phase.

Starting in West Africa is also a quality-control decision. The main risk in a small fine-tuned model is plausible invention, and we can check output against places we actually know. It keeps the scope honest for a 15-day build, and the pipeline is region-centric, so the same scripts extend outward once the method is proven.

Running on an open-weight model keeps inference free — which matters for users and institutions in the region.

## What the model does

**Input:** a raw description of a West African destination, plus an instruction.

**Output:** one of three things, depending on the instruction — structured labels (theme, country, terrain), a short traveller digest, or a grounded answer to a follow-up question. Out-of-scope logistics questions get a refusal.

### Example

**Input**

```
classify this:

A UNESCO passage: "The Osun-Osogbo Sacred Grove, on the outskirts of
Osogbo in south-west Nigeria, is a dense forest along the banks of the
Osun river. It contains sanctuaries, shrines, sculptures and artworks
in honour of Osun and other Yoruba deities."
```

**Output**

```json
{
  "country": "Nigeria",
  "theme": "Sacred & Cultural Landscape",
  "terrain": "Riverine forest",
  "traveler_profile": "Culture-focused, slow travel"
}
```

## Scope

**In scope** — the discovery layer: classification, summarization and grounded Q&A over West African destinations.

**Out of scope**

- Guide onboarding, bookings and payments — a later phase
- Anything outside West Africa
- Live logistics: visa fees, border closures, flight costs, medical requirements, security alerts. The model declines these and points to official sources.
- External retrieval or search. This is Gemma fine-tuning only.

## Data

| | |
|---|---|
| **Types** | Text, images, tabular (spreadsheets, numbers). Research may surface unusual dataset forms; we intend to use whatever we find. |
| **Sources** | An existing dataset, data we collect ourselves, and synthetic data we generate |
| **Language** | English |
| **Target size** | ~3,500 examples |
| **Licence & consent** | Confirmed |

### Known coverage gaps

- Travellers with disabilities
- West African countries with little written material online
- Non-English speakers and readers

## Model

**Gemma 4 E4B**, fine-tuned with **QLoRA** (4-bit, via PEFT).

## Evaluation

Measured on a held-out set with **accuracy, F1, recall, ROUGE-L** and **factual consistency**.

**Success target:** 90% of the claims in the model's answers and summaries are supported by the source passage.

## Impact

**Main benefits**

- Travellers and tourists get accurate answers instead of guesses
- Lesser-known West African heritage sites become easy to find and understand
- The open dataset and model let others build on the work and extend it to other regions

**Main risk**

The model could state wrong facts or misrepresent a community's culture, and users could mistake its output for current travel or safety advice.

### Stakeholders

| Group | What matters to them | Risks they face | Power | How we involve them |
|---|---|---|---|---|
| Independent travellers, domestic tourists, students, cultural researchers | Accurate, easy-to-read information; discovering new places | Acting on wrong facts | Medium | Disclaimers in the interface |
| Communities whose places are described; national museum and monument bodies | Being portrayed accurately and respectfully | Misrepresentation, stereotyping, exposure of restricted cultural knowledge | Low | A way to report errors or request removal in the demo |
| Wikipedia, Wikivoyage and Wikimedia Commons editors; UNESCO | Proper credit | Reuse of their work without attribution | Medium | Attribution on every passage and image |
| Open-source AI community | Reusable, well-documented data and models | Inheriting hidden biases or coverage gaps | Low | Published data card, model card and code |

## Governance

Our main risk is reduced through **retrieval grounding, human review and clear disclaimers**.

- We do not collect or store personal data, and error reports keep no personal details.
- UNESCO terms of use are adhered to.
- We follow the data protection acts of all countries covered.
- The model must never be used for travel safety, security, visa, health or border advice.

**Accountability.** The team is accountable, with one named owner. That owner holds the Hugging Face Space and the repository and can take the demo offline within minutes — the kill switch is deleting the Space, since the adapter is ours alone.

**Reporting.** Reports arrive through the repository community tab and a contact address in the model card. We acknowledge within 48 hours, verify the claim against its source passage, and publish the known failure in the model card the same week.

## Environmental impact

- Smallest Gemma variant that clears our evaluation targets, fine-tuned with QLoRA 4-bit through PEFT — a few hours on one consumer GPU instead of days on a multi-GPU node, producing adapter weights of a few hundred megabytes rather than a full checkpoint.
- Training runs capped at one baseline plus three hyperparameter passes. No open-ended sweeps.
- Synthetic generation runs once over 800–1,200 passages and is cached, so no passage reaches the teacher model twice.
- Inference serves adapters on a shared Spaces instance that sleeps when idle, not a dedicated always-on server.

## Team

**Team Ijeoma AI** — Nigeria Cohort 1

- **John Hicks** — Teaching Assistant, University of Lagos *(submitter)*
- **Prof. Chika Yinka-Banjo** — AI Champion, University of Lagos
- **Usman Kolawole** — Teaching Assistant, University of Lagos

**Pathway:** Your own path · **Date:** 1 November 2026

---

## Disclaimer

Ijeoma AI is a research prototype for destination discovery. It is **not** a source of travel safety, security, visa, health or border advice. Verify all practical travel information with official sources.
