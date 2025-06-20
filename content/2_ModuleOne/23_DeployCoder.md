---
title: "Deploy Coder Control Plane" 
chapter: true
weight: 33 
---

# Deploy Coder Control Plane 

## Configure and Deploy Coder with AWS Load Balancer Integration

The Coder control plane will be deployed to your EKS cluster using Helm, with an AWS Network Load Balancer providing external access. This setup ensures high availability and proper integration with AWS services.

{{% notice info %}}
The deployment process involves two phases: initial deployment and configuration update once the load balancer is provisioned.
{{% /notice %}}

#### Step 1: Download Coder Helm Values Configuration

First, download the pre-configured Helm values file for this workshop:

```bash
# Download the Coder configuration file
curl -o coder-core-values-v2.yaml https://raw.githubusercontent.com/your-workshop-repo/main/static/infrastructure/coder-core-values-v2.yaml

# Verify the file was downloaded
ls -la coder-core-values-v2.yaml
```

#### Step 2: Install Coder Control Plane

```bash
# Add Coder Helm repository
helm repo add coder-v2 https://helm.coder.com/v2
helm repo update

# Install Coder using the configuration file
helm install coder coder-v2/coder \
    --namespace coder \
    --values coder-core-values-v2.yaml \
    --version 2.15.1
```

{{% notice tip %}}
You can check the latest Coder version at the [Coder Releases Page](https://github.com/coder/coder/releases). Version 2.15.1 is known to work well with this workshop configuration.
{{% /notice %}}

#### Step 3: Monitor Deployment Progress

Wait for the Coder deployment to complete and the AWS Load Balancer to be provisioned:

```bash
# Watch the Coder pod startup
kubectl get pods -n coder -w

# Check the service and wait for EXTERNAL-IP to be assigned
kubectl get service coder -n coder -w
```

This process typically takes 3-5 minutes. You'll see the EXTERNAL-IP change from `<pending>` to an AWS load balancer hostname.

#### Step 4: Retrieve Load Balancer Information

Once the load balancer is ready, capture the external endpoint:

```bash
# Get the load balancer hostname
CODER_LB_HOSTNAME=$(kubectl get service coder -n coder -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
echo "Coder Load Balancer: $CODER_LB_HOSTNAME"

# Construct the access URLs
CODER_ACCESS_URL="http://$CODER_LB_HOSTNAME"
CODER_WILDCARD_URL="*.$CODER_LB_HOSTNAME"

echo "Coder Access URL: $CODER_ACCESS_URL"
echo "Coder Wildcard URL: $CODER_WILDCARD_URL"
```

#### Step 5: Update Coder Configuration with Load Balancer URLs

Update the Coder configuration with the actual load balancer endpoints:

```bash
# Update the values file with the real URLs
sed -i "s|https://coder.example.com|$CODER_ACCESS_URL|g" coder-core-values-v2.yaml
sed -i "s|\*.coder.example.com|$CODER_WILDCARD_URL|g" coder-core-values-v2.yaml

# Apply the updated configuration
helm upgrade coder coder-v2/coder \
    --namespace coder \
    --values coder-core-values-v2.yaml \
    --version 2.15.1
```

#### Step 6: Verify Coder Accessibility

Test that Coder is accessible via the load balancer:

```bash
# Test HTTP connectivity
curl -I $CODER_ACCESS_URL/healthz

# If successful, you should see HTTP 200 response
```

{{% notice success %}}
If you see `HTTP/1.1 200 OK`, Coder is successfully deployed and accessible!
{{% /notice %}}

#### Step 7: Access Coder Web Interface

You can now access Coder in your browser:

{{% button href="#" icon="fas fa-external-link-alt" %}}Open Coder Dashboard{{% /button %}}

```bash
# Print the URL for easy copying
echo "Access Coder at: $CODER_ACCESS_URL"
```

When you first access Coder, you'll be prompted to:
1. Create an admin account
2. Set up your organization
3. Configure authentication (if desired)

## Troubleshooting

{{% expand "Coder pod not starting" %}}
Check the pod logs and events:
```bash
kubectl describe pod -n coder -l app.kubernetes.io/name=coder
kubectl logs -n coder -l app.kubernetes.io/name=coder
```

Common issues:
- Database connection problems (check PostgreSQL pod status)
- Insufficient resources (check node capacity)
- Configuration errors (validate the values file)
{{% /expand %}}

{{% expand "Load balancer not getting external IP" %}}
Verify AWS Load Balancer Controller is installed:
```bash
kubectl get pods -n kube-system | grep aws-load-balancer-controller
```

Check service events:
```bash
kubectl describe service coder -n coder
```

The EKS cluster should have the AWS Load Balancer Controller installed automatically with Auto Mode.
{{% /expand %}}

{{% expand "Cannot access Coder web interface" %}}
Verify the service and endpoints:
```bash
kubectl get service coder -n coder
kubectl get endpoints coder -n coder
```

Check security groups allow traffic on port 80:
```bash
# The load balancer security group should allow inbound HTTP traffic
aws ec2 describe-security-groups --filters "Name=group-name,Values=*coder*"
```
{{% /expand %}}

### Next Steps
With Coder successfully deployed and accessible, you're ready to configure IAM roles for workspace provisioning and set up development templates.