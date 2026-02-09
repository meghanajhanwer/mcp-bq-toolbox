```bash
gcloud builds submit --project=crafty-plateau-484419-m9 --region=us-central1 --config=cloudbuild.yaml .
```

Testing from local

```bash
gcloud run services proxy toolbox-mcp --region us-central1 --port 8080  
```


```bash
gcloud auth list
gcloud config set account USER_EMAIL

PROJECT=crafty-plateau-484419-m9
REGION=us-central1
SERVICE=toolbox-mcp

URL=$(gcloud run services describe "$SERVICE" \
  --project="$PROJECT" --region="$REGION" \
  --format='value(status.url)')

TOKEN=$(gcloud auth print-identity-token)

```

# Run SELECT (dry run) through MCP tool
curl -s -X POST "$URL/mcp" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d @- <<'JSON'
{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "tools/call",
  "params": {
    "name": "marketing_sql",
    "arguments": {
      "sql": "SELECT table_name FROM `crafty-plateau-484419-m9.marketing_demo.INFORMATION_SCHEMA.TABLES` LIMIT 5",
      "dry_run": true
    }
  }
}
JSON
