# Azure AI Services Monitoring Lab

A step-by-step guide for monitoring Azure AI Services, including setting up alerts and visualizing metrics.

## Overview

This lab demonstrates how to:
- Monitor Azure AI Services activity
- Configure alerts for resource usage  
- Visualize service metrics
- Set up diagnostic logging

## Prerequisites

- Azure subscription
- Azure CLI installed
- Git
- Visual Studio Code (optional)

## Lab Steps

### 1. Create Azure AI Services Resource

1. Log into [Azure Portal](https://portal.azure.com)
2. Create a new **Azure AI services multi-service account**
3. Note your resource name and resource group

### 2. Get Service Credentials

List your service keys:
```bash
az cognitiveservices account keys list \
  --name <your-resource-name> \
  --resource-group <your-resource-group>
```

Get your endpoint:
```bash
az cognitiveservices account show \
  --name <your-resource-name> \
  --resource-group <your-resource-group> \
  --query 'properties.endpoint' -o tsv
```

### 3. Configure Alerts

1. In Azure Portal, navigate to your AI Services resource
2. Go to **Monitoring** → **Alerts**
3. Click **+ Create** → **Alert rule**
4. Configure:
   - **Condition**: List Keys
   - **Threshold**: Count > 1
   - **Alert rule name**: "List Keys Alert"
5. Save the alert rule

### 4. Generate Test Traffic

Create a test file (`test_sentiment.json`):
```json
{
  "kind": "SentimentAnalysis",
  "parameters": {
    "modelVersion": "latest"
  },
  "analysisInput": {
    "documents": [
      {
        "id": "1",
        "language": "en",
        "text": "I had a wonderful trip to Seattle last week!"
      }
    ]
  }
}
```

Send test requests:
```bash
# Single request
curl -X POST "https://<your-endpoint>/language/:analyze-text?api-version=2023-04-01" \
  -H "Content-Type: application/json" \
  -H "Ocp-Apim-Subscription-Key: <your-key>" \
  --data @test_sentiment.json

# Multiple requests (generates metrics)
for i in {1..5}; do
  echo "Request $i"
  curl -sX POST "https://<your-endpoint>/language/:analyze-text?api-version=2023-04-01" \
    -H "Content-Type: application/json" \
    -H "Ocp-Apim-Subscription-Key: <your-key>" \
    --data @test_sentiment.json
  sleep 2
done
```

### 5. View Metrics

1. In Azure Portal, go to your resource
2. Navigate to **Monitoring** → **Metrics**
3. Select **Total Calls** metric
4. View the activity graph
5. Try different:
   - Time ranges
   - Aggregation types
   - Filters (API Name, Model)

### 6. Create Monitoring Script

Create `monitor-ai-service.sh`:
```bash
#!/bin/bash
# Azure AI Services monitoring script

ENDPOINT="https://<your-endpoint>"
KEY="<your-key>"

# Test different APIs
echo "Testing Language Detection..."
curl -sX POST "$ENDPOINT/text/analytics/v3.1/languages?" \
  -H "Content-Type: application/json" \
  -H "Ocp-Apim-Subscription-Key: $KEY" \
  --data '{"documents":[{"id":"1","text":"Hello world"}]}'

echo -e "\n\nTesting Sentiment Analysis..."
curl -sX POST "$ENDPOINT/language/:analyze-text?api-version=2023-04-01" \
  -H "Content-Type: application/json" \
  -H "Ocp-Apim-Subscription-Key: $KEY" \
  --data @test_sentiment.json
```

### 7. Optional: Configure Diagnostic Logging

1. In your resource, go to **Monitoring** → **Diagnostic settings**
2. Click **+ Add diagnostic setting**
3. Configure:
   - Select logs to collect
   - Choose destination (Storage/Log Analytics/Event Hub)
   - Save settings

## Important Security Notes

- **Never commit credentials** to source control
- Use environment variables or Azure Key Vault for keys
- Rotate keys regularly
- Monitor access patterns for security

## Clean Up Resources

To avoid charges:
1. Go to Azure Portal
2. Navigate to your resource group
3. Delete the resource group
4. Confirm deletion

## Additional Resources

- [Azure AI Services Documentation](https://docs.microsoft.com/azure/cognitive-services/)
- [Azure Monitor Documentation](https://docs.microsoft.com/azure/azure-monitor/)
- [Azure CLI Reference](https://docs.microsoft.com/cli/azure/)

## License

This lab guide is provided as-is for educational purposes.

---

*Created as part of Azure AI Services learning exercises*