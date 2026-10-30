# Changelog

All notable changes to `forkit-core` will be documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
This project uses [Semantic Versioning](https://semver.org/).

---

## [Unreleased]

### Fixed

- `verify_passport_id` now normalises uppercase `artifact_hash` and stored `id`
  before comparison, matching the identity specification. Fixes #10.

---

## [0.1.0] — 2026-03-16

Initial public release.

### Added

**Core domain (`forkit.domain`)**
- `identity.py` — `compute_id`, `validate_hash`, `validate_version`, `to_json_safe`. Canonical, offline-first passport ID derivation via SHA-256.
- `hashing.py` — `HashEngine` with `hash_file`, `hash_directory`, `hash_dict`, `hash_string`, `hash_bytes`, `hash_artifact`, `hash_config`, `hash_system_prompt`, `hash_metadata`; corresponding `verify_*` methods; strict `is_valid_hash`.
- `lineage.py` — `LineageGraph` DAG with DFS cycle detection, `ancestors`, `descendants`, `edges_for`, `nodes_by_type`; `register_model` / `register_agent` convenience constructors; JSON save/load.
- `integrity.py` — `verify_passport_id`, `compute_metadata_hash`.

**Schemas (`forkit.schemas`)**
- `ModelPassport` — identity and provenance document for an AI model. Fields include `artifact_hash`, `parent_hash`, `base_model_id`, `base_model_name`, `fine_tuning_method`, `training_data`, `capabilities`, `parameter_count`.
- `AgentPassport` — identity and provenance document for an AI agent. Fields include `model_id` (hard link to `ModelPassport`), `system_prompt`, `tools`, `parent_agent_id`, `endpoint_hash`, `fork_reason`.
- Auto-selects Pydantic v2 backend when available; falls back to pure-Python dataclasses (zero hard dependencies).
- Shared Pydantic inner models extracted to `pydantic/_types.py` — no duplication between model and agent backends.
- Full `to_dict` / `from_dict` serialisation roundtrip with enum and datetime coercion.
- `_PYDANTIC_AVAILABLE` flag exported from `forkit.schemas`.

**Registry (`forkit.registry`)**
- `LocalRegistry` — filesystem-backed store (JSON files + SQLite index).
- `register_model`, `register_agent`, `get`, `delete`, `list`, `search`.
