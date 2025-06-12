# 📊 Run Evidently Report GitHub Action

[![GitHub Action](https://img.shields.io/badge/Action-Evidently-blue?logo=githubactions)](https://github.com/evidentlyai/evidently-report-action)

A flexible, CI-friendly GitHub Action to run Evidently's CLI for data and ML model monitoring reports.  
This action integrates Evidently's evaluation workflows into your CI pipelines with options for local and cloud storage, test summaries, smart result links, and optional GitHub Check Runs.

---

## 📖 Overview

This action runs [Evidently CLI](https://github.com/evidentlyai/evidently) reports using a configuration file and dataset(s) provided either locally or from [Evidently Cloud](https://app.evidently.cloud). It supports:

✅ Running descriptors, metrics, and tests  
✅ Failing the job if any test fails  
✅ Saving results locally or uploading to Evidently Cloud  
✅ Auto-linking results via workflow notices or GitHub Check Runs  
✅ Optional artifact uploads of result files  

---

## ⚡️ Quickstart

```yaml
- uses: actions/checkout@v4

- uses: evidentlyai/evidently-report-action@v1
  with:
    config_path: "evidently_config.json"
    input_path: "data/current.csv"
    reference_path: "data/reference.csv"
    output: "reports/run-${{ github.sha }}"
    api_key: ${{ secrets.EVIDENTLY_API_KEY }}
````

---

## 📥 Inputs

| Name               | Description                                                 | Required | Default                  |
|:-------------------|:------------------------------------------------------------|:---------|:-------------------------|
| `config_path`      | Path to Evidently report config file (JSON or Python `.py`) | ✅        | —                        |
| `input_path`       | URI to current dataset (local file or `cloud://` URI)       | ❌        | —                        |
| `reference_path`   | URI to reference dataset (optional)                         | ❌        | —                        |
| `output`           | Output URI for saving results (local or `cloud://` URI)     | ❌        | `evidently_report_<sha>` |
| `dataset_name`     | Dataset name when no metrics in config (default: `CLI run`) | ❌        | `CLI run`                |
| `test_summary`     | Run test summary and fail CI on test failure                | ❌        | `true`                   |
| `save_dataset`     | Save dataset snapshot                                       | ❌        | `true`                   |
| `save_report`      | Save full report files                                      | ❌        | `true`                   |
| `project_id`       | Evidently Cloud project ID (if saving to cloud)             | ❌        | —                        |
| `dataset_id`       | Evidently Cloud dataset ID (if loading from cloud)          | ❌        | —                        |
| `base_url`         | Evidently Cloud base URL                                    | ❌        | `app.evidently.cloud`    |
| `upload_artifacts` | Upload local result files as GitHub Action artifacts        | ❌        | `false`                  |
| `api_key`          | Evidently Cloud API key (for cloud storage access)          | ❌        | —                        |
| `use_pip_cache`    | Cache pip dependencies between runs                         | ❌        | `true`                   |
| `check-link`       | Create GitHub Check Run with external link if available     | ❌        | `true`                   |

---

## 📤 Outputs

| Name         | Description                                                |
| :----------- | :--------------------------------------------------------- |
| `exit_code`  | Exit code returned by Evidently CLI                        |
| `found_link` | URL to the saved report or dataset snapshot (if available) |

---

## 📦 Example: Python Config

You can define your report config in a Python file using Evidently’s API:

```python
from evidently.cli.report import ReportConfig

conf = ReportConfig(
    descriptors=[
        # list of descriptors
    ],
    metrics=[
        # list of metrics
    ]
)

conf.save("evidently_config.json")  # Save as JSON if you need a config file
```

Alternatively, you can use the Python file directly without saving it:

```yaml
- uses: evidentlyai/evidently-report-action@v1
  with:
    config_path: "evidently_config.py:conf"
```

Where `conf` is the name of the config object in the Python file.

---

## 📝 Notes

* **Cloud support** requires an Evidently Cloud account and a valid API key.
* If `test_summary` is enabled and any test fails, the action will fail the workflow with exit code `1`.
* When `upload_artifacts` is enabled and no cloud link is found, result files from the output path are uploaded as workflow artifacts.
* If `check-link` is enabled and a result link is extracted, a GitHub Check Run is created pointing to it (requires permissions).

---

## 🔒 Required Permissions

For the action to function properly (especially for Check Runs and linking), your workflow should declare at least:

```yaml
permissions:
  checks: write
  contents: read
  pull-requests: read
  actions: read
```

---

## 📌 Related Links

* [Evidently Documentation](https://docs.evidentlyai.com/)
* [Evidently Cloud](https://app.evidently.cloud)
* [Evidently CLI](https://github.com/evidentlyai/evidently)
* [Action Source Code](https://github.com/evidentlyai/evidently-report-action)


