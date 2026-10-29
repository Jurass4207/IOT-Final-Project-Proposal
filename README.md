# Taiwan Air Quality Evidence Agent

台灣空氣品質證據分析代理人：以公開逐時監測資料，回答特定地點與時段的空品趨勢、事件與測站差異，並附上觀測值、時間、來源和限制。

## Project status

Proposal-stage working draft. The problem framing and system design are documented; the first data source is identified. API access, historical coverage, geographic scope, literature review, experiments, and implementation remain to be verified or completed.

## Project files

- `Proposal.md` — Abstract, Introduction, Related Work, Deep Research, Proposed Scheme, and initial references.
- `DESIGN.md` — Architecture, components, data flow, domain assumptions, algorithm, implementation plan, evaluation, and limitations.
- `research/data-source-inventory.md` — Candidate data sources, fields, access questions, and initial canonical schema.
- `output/Proposal_v2.pptx` — 12-slide Proposal presentation draft.

## Primary data source

The MVP will use the Taiwan Ministry of Environment historical hourly AQI dataset, AQX_P_488: <https://data.moenv.gov.tw/dataset/detail/aqx_p_488>. The platform lists monthly historical archives. A separate current hourly dataset, AQX_P_432, can support a live-data demonstration: <https://data.moenv.gov.tw/dataset/detail/aqx_p_432>. Both include station identifiers and locations, AQI/status, pollutant observations, wind fields, and publication time.

The Ministry's air-pollution IoT observations are a possible finer-resolution extension: <https://env.moenv.gov.tw/en/Issue/Index/53b6ae55-bb69-449e-953e-01c2330ca3b8>. Sensor integration depends on verifying API access, historical retention, station correspondence, and quality metadata.

## Data and evidence principles

- Preserve source records and retrieval time; document timezone and units.
- Treat missing, stale, duplicate, or conflicting values as quality flags.
- Keep station observations distinct from personal exposure.
- Do not claim pollutant causes or individualized health outcomes without appropriate evidence.
- Never commit API credentials. Store them in a local environment variable or an ignored configuration file.

## How to run

Not implemented yet. The historical download/API pathway and the implementation stack will be recorded here after programmatic access and data continuity are checked.

## License and attribution

The project will follow the source dataset's current open-data license and include attribution for all datasets and research sources. Confirm exact license terms before distributing any data extracts.
