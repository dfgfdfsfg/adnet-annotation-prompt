# ADNet Text Annotation Prompt Template

## Purpose

ADNet uses an LLM-assisted pipeline to generate structured textual descriptions for anomalous samples. The prompt is instantiated with the corresponding domain, class name, defect type, and available visual or mask cues. The model is required to return a fixed six-field JSON object.

## Input Fields

- `domain`: ADNet domain name, such as `Electronics`, `Industry`, `Agrifood`, `Infrastructure`, or `Medical`.
- `class_name`: ADNet category name.
- `defect_type`: defect type or anomaly subtype when available.
- `visual_cues`: optional visible evidence or mask-derived cues.

## Prompt Template

```text
You are an anomaly detection annotation assistant.

Given an anomalous image from the following anomaly detection category:
- Domain: {domain}
- Class name: {class_name}
- Defect type: {defect_type}
- Optional visual or mask cues: {visual_cues}

Describe the anomaly using concise visual inspection terminology.

Requirements:
1. Return valid JSON only.
2. Use the following fields:
   location, color, shape, area_size, quantity, reason.
3. Describe only visible or strongly supported evidence.
4. Do not invent unsupported causes or material properties.
5. Use "unknown" when the evidence is insufficient.

JSON schema:
{
  "location": "...",
  "color": "...",
  "shape": "...",
  "area_size": "...",
  "quantity": "...",
  "reason": "..."
}
```

## Required JSON Output

The generated annotation must be a single JSON object with these fields:

```json
{
  "location": "top-left",
  "color": "dark brown",
  "shape": "irregular spot",
  "area_size": "small",
  "quantity": "1",
  "reason": "unknown"
}
```

## Quality Control

Generated JSON files are checked automatically for parse validity and required fields. Manual verification is then performed across all five ADNet domains to correct malformed entries, inconsistent terminology, overly generic descriptions, and unsupported causal statements.

## Release Files

- `adnet_annotation_schema.json`: machine-readable JSON Schema for validating annotation records.
- `example_annotation_record.json`: example record following the schema.
