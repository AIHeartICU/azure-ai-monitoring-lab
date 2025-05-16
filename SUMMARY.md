# Lab Summary: Azure AI Services Monitoring

## What We Did

1. **Listed Azure AI Service Keys**
   - Used Azure CLI to retrieve service keys
   - Command: `az cognitiveservices account keys list`

2. **Retrieved Service Endpoint**
   - Got the endpoint URL for API calls
   - Command: `az cognitiveservices account show`

3. **Set Up Monitoring Alert**
   - Created alert rule in Azure Portal for "List Keys" activity
   - Configured to trigger when keys are accessed

4. **Generated Test Traffic**
   - Created JSON file for sentiment analysis testing
   - Made multiple API calls to generate metrics data
   - Used curl commands to interact with the service

5. **Created Monitoring Script**
   - Built reusable shell script for testing different APIs
   - Included language detection and sentiment analysis tests

6. **Published to GitHub**
   - Created clean, public-ready documentation
   - Removed all sensitive information
   - Published to: https://github.com/AIHeartICU/azure-ai-monitoring-lab

## Key Learnings

- How to monitor Azure AI Services usage
- Setting up alerts for resource activity
- Visualizing metrics in Azure Portal
- Creating test scripts for API validation
- Best practices for security (no hardcoded credentials)

## Next Steps

- Configure diagnostic logging for detailed insights
- Set up automated monitoring workflows
- Integrate with Azure Monitor dashboards
- Create production-ready monitoring solutions

---

*This lab guide is now available for CuraTrak integration and other projects.*