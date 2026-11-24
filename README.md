# hss-biblio-mcp

A layered, query-economical MCP (Model Context Protocol) stdio server for **Humanities & Social Sciences** book lists, syllabus-style reading lists, and multilingual literature discovery — bridging DOI-indexed graphs (OpenAlex, Crossref, Semantic Scholar) with print-first Continental European, Latin American, and independent open-access presses (K10plus Books/Articles/Reviews, IAI Berlin, Lobid, BnF, BNE, OPAC SBN Italy, Thoth, OpenAIRE Graph, DOAJ, EconBiz, HAL, theses.fr, LA Referencia, GoTriple, optional CORE).

面向人文社科研究的**分层递进、高经济性多语种书单（Book List）与文献单（Literature List）**检索 MCP 服务——深度整合 **英美主流学术库 + 欧陆（德/法/意/西）国家馆藏与联合目录 + 拉美开放学术库 + 独立开放获取小出版社平台**，内建 SQLite 本地按源缓存、短句柄复用（`item_id`）、熔断保护、原生语种直达路由、近五年最新动向双轨并发、OpenAlex 原生期刊解析、多学派并行引文雪球与反例/批判探针。

---

## Supported Bibliographic Data Sources

| Language / Scope | Books & Union Catalogs | Articles, Chapters, Reviews & Theses |
| :--- | :--- | :--- |
| **Global / Anglo** | OpenAlex, Open Library, Crossref, Thoth (OA), Google Books (optional) | OpenAlex, Semantic Scholar (optional), CORE (optional) |
| **German (`de`)** | K10plus (`opac-de-627`), Lobid (hbz), DNB | K10plus Articles & Reviews, EconBiz (ZBW), OpenAIRE Graph, DOAJ |
| **French (`fr`)** | BnF (SRU), K10plus | HAL / HAL-SHS, theses.fr, OpenAIRE Graph, DOAJ, GoTriple |
| **Italian (`it`)** | OPAC SBN (ICCU), K10plus, Lobid | OpenAIRE Graph (Italian IRIS repos), DOAJ, GoTriple |
| **Spanish / LatAm (`es`, `pt`)** | BNE (Alma SRU), IAI Berlin (`opac-de-204`), K10plus | DOAJ, OpenAIRE Graph (RECOLECTA), LA Referencia, GoTriple |

---

## 4-Layer Progressive Workflow

| Layer | Tool | Key Capabilities |
| :--- | :--- | :--- |
| **Layer 1** | `topic_search` | Dual-track discovery (`consensus` / `specialist` + `recent_frontiers` for last 5 years), sub-dimension tagging (`dimension`), server-side journal resolution (`venues`), and native multilingual query routing (`queries_by_language`). |
| **Layer 2** | `deep_dive` | Zero-network `inspect`, native `venue` filtering, `counter_narratives` probing, multi-seed citation snowballing (`bridge_works`), plus `series`, `chapters`, `reviews`, and `author` deep dives. |
| **Layer 3** | `build_reading_list` | 5-unit syllabus assembly (Foundations, Core Monographs, Regional & Chapters, Debates & Critiques, Recent Frontiers) with optional `group_by_dimensions=True`. |
| **Layer 4** | `verify_citations`, `format_bibliography`, `get_economy_status` | Zero-hallucination verification (DOI / ISBN / print catalog), zero-request local bibliography export (Chicago / APA / MLA / BibTeX), and per-source query economy & quota telemetry. |

---

## Installation & Configuration

```bash
python -m venv .venv
pip install -e ".[dev]"
```

Configure in your MCP client config (see `mcp_config.example.json`):

```json
{
  "mcpServers": {
    "hss-biblio": {
      "command": "python",
      "args": ["-m", "hss_biblio_mcp"],
      "env": {
        "HSS_CONTACT_EMAIL": "your-email@example.com",
        "OPENALEX_API_KEY": "",
        "SEMANTIC_SCHOLAR_API_KEY": "",
        "GOOGLE_BOOKS_API_KEY": "",
        "CORE_API_KEY": "",
        "OPENAIRE_API_TOKEN": "",
        "HSS_CACHE_TTL_SECONDS": "86400"
      }
    }
  }
}
```

## Testing

```bash
pytest -v tests/test_unit.py
```
