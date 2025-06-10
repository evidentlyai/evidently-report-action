# 📊 Evidently Report GitHub Action

Reusable GitHub Action for running Evidently CLI reports with local and cloud output options.

## Usage

```yaml
- uses: yourusername/evidently-report-action@v1
  with:
    config_path: "config/report.yaml"
    api_key: ${{ secrets.EVIDENTLY_API_KEY }}
    upload_artifacts: "true"