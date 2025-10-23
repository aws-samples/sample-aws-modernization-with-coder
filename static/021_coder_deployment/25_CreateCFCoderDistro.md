---
title: "Create CloudFront Distribution"
chapter: false
weight: 35
---

## Configure CloudFront for Global Access and TLS Termination

CloudFront provides global content delivery, TLS termination, and improved performance for your Coder deployment. This module will create a CloudFront distribution that sits in front of your AWS Load Balancer and updates the Coder configuration to use HTTPS.

::alert[CloudFront distributions can take 10-15 minutes to deploy globally. The process involves creating the distribution, waiting for deployment, and then updating the Coder configuration.]{type="info"}

#### Step 1: Retrieve Current Load Balancer Information

First, get the current load balancer hostname from your Coder service:

```bash
# Get the current load balancer hostname
CODER_LB_HOSTNAME=$(kubectl get service coder -n coder -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
echo "Current Coder Load Balancer: $CODER_LB_HOSTNAME"

# Verify the load balancer is accessible
curl -I http://$CODER_LB_HOSTNAME
```

#### Step 2: Create CloudFront Distribution

Create a CloudFront distribution using the AWS CLI:

```bash
# Create distribution configuration
cat > cloudfront-config.json << EOF
{
  "CallerReference": "coder-workshop-$(date +%s)",
  "Comment": "Coder Workshop CloudFront Distribution",
  "DefaultCacheBehavior": {
    "TargetOriginId": "coder-alb-origin",
    "ViewerProtocolPolicy": "redirect-to-https",
    "AllowedMethods": {
      "Quantity": 7,
      "Items": ["GET", "HEAD", "OPTIONS", "PUT", "POST", "PATCH", "DELETE"],
      "CachedMethods": {
        "Quantity": 2,
        "Items": ["GET", "HEAD"]
      }
    },
    "ForwardedValues": {
      "QueryString": true,
      "Cookies": {"Forward": "all"},
      "Headers": {
        "Quantity": 1,
        "Items": ["*"]
      }
    },
    "MinTTL": 0,
    "DefaultTTL": 0,
    "MaxTTL": 0
  },
  "Origins": {
    "Quantity": 1,
    "Items": [
      {
        "Id": "coder-alb-origin",
        "DomainName": "$CODER_LB_HOSTNAME",
        "CustomOriginConfig": {
          "HTTPPort": 80,
          "HTTPSPort": 443,
          "OriginProtocolPolicy": "http-only"
        }
      }
    ]
  },
  "Enabled": true,
  "PriceClass": "PriceClass_100"
}
EOF

# Create the CloudFront distribution
CLOUDFRONT_RESULT=$(aws cloudfront create-distribution --distribution-config file://cloudfront-config.json)
echo "$CLOUDFRONT_RESULT" > cloudfront-result.json

# Extract distribution information
DISTRIBUTION_ID=$(echo "$CLOUDFRONT_RESULT" | jq -r '.Distribution.Id')
CLOUDFRONT_DOMAIN=$(echo "$CLOUDFRONT_RESULT" | jq -r '.Distribution.DomainName')

echo "CloudFront Distribution ID: $DISTRIBUTION_ID"
echo "CloudFront Domain: $CLOUDFRONT_DOMAIN"
```

#### Step 3: Wait for CloudFront Deployment

Monitor the CloudFront distribution deployment status:

```bash
# Check deployment status
echo "Waiting for CloudFront distribution to deploy..."
while true; do
  STATUS=$(aws cloudfront get-distribution --id $DISTRIBUTION_ID --query 'Distribution.Status' --output text)
  echo "Distribution Status: $STATUS"
  if [ "$STATUS" = "Deployed" ]; then
    echo "CloudFront distribution is ready!"
    break
  fi
  echo "Waiting 60 seconds before checking again..."
  sleep 60
done
```

::alert[CloudFront deployment typically takes 10-15 minutes. You can continue with other tasks and return to check the status periodically.]{type="info"}

#### Step 4: Test CloudFront Distribution

Verify the CloudFront distribution is working:

```bash
# Test HTTPS access through CloudFront
curl -I https://$CLOUDFRONT_DOMAIN

# Test HTTP redirect to HTTPS
curl -I http://$CLOUDFRONT_DOMAIN
```


You should see HTTP 301/302 redirects for HTTP requests and successful responses for HTTPS requests.


#### Step 5: Update Coder Configuration with CloudFront URLs

Update the Coder Helm values to use the CloudFront distribution:

```bash
# Construct the new URLs
CODER_CLOUDFRONT_URL="https://$CLOUDFRONT_DOMAIN"
CODER_CLOUDFRONT_WILDCARD="*.$CLOUDFRONT_DOMAIN"

echo "New Coder Access URL: $CODER_CLOUDFRONT_URL"
echo "New Coder Wildcard URL: $CODER_CLOUDFRONT_WILDCARD"

# Update the values file with CloudFront URLs
sed -i "s|http://$CODER_LB_HOSTNAME|$CODER_CLOUDFRONT_URL|g" coder-core-values-v2.yaml
sed -i "s|\*\.$CODER_LB_HOSTNAME|$CODER_CLOUDFRONT_WILDCARD|g" coder-core-values-v2.yaml

sed -i "s|https://coder.example.com|$CODER_CLOUDFRONT_URL|g" coder-core-values-v2.yaml
sed -i "s|\*.coder.example.com|$CODER_CLOUDFRONT_WILDCARD|g" coder-core-values-v2.yaml


# Apply the updated configuration
helm upgrade coder coder-v2/coder \
    --namespace coder \
    --values coder-core-values-v2.yaml \
    --version 2.24.3
```

#### Step 6: Verify Coder HTTPS Access

Test that Coder is accessible via HTTPS through CloudFront:

```bash
# Test HTTPS connectivity
curl -I $CODER_CLOUDFRONT_URL

# Print the final URL for browser access
echo "Access Coder securely at: $CODER_CLOUDFRONT_URL"
```


If you see **HTTP/2 200** or **HTTP/1.1 200 OK**, Coder is successfully accessible via HTTPS through CloudFront!


## Troubleshooting


Check your AWS permissions and region:
```bash
# Verify CloudFront permissions
aws cloudfront list-distributions

# Check if the load balancer hostname is valid
nslookup $CODER_LB_HOSTNAME
```

Ensure your IAM role has CloudFront permissions:
- `cloudfront:CreateDistribution`
- `cloudfront:GetDistribution`
- `cloudfront:UpdateDistribution`



Verify the origin configuration:
```bash
# Check if the load balancer is responding
curl -v http://$CODER_LB_HOSTNAME

# Verify CloudFront origin settings
aws cloudfront get-distribution-config --id $DISTRIBUTION_ID
```

Common issues:
- Origin protocol policy mismatch
- Load balancer security group blocking CloudFront IPs
- Origin domain name incorrect



Check the Coder pod logs and configuration:
```bash
# Check Coder pod status
kubectl get pods -n coder
kubectl logs -n coder -l app.kubernetes.io/name=coder

# Verify the updated configuration
helm get values coder -n coder
```

Ensure the access URL in the Helm values matches the CloudFront domain.


### Next Steps
With CloudFront successfully configured, your Coder deployment now has:
- Global content delivery and improved performance
- Automatic HTTPS/TLS termination
- Enhanced security through AWS's edge locations

You're ready to configure development templates and user workspaces. Remember to bookmark your **Coder CloudFront URL** for easy reference for the remainder of the workshop.