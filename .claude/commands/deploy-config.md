---
allowed-tools: Write(*),Read(*)
description: Generate deployment configurations for Docker, Kubernetes, and CI/CD pipelines
---

# Deploy Config Command

Generate deployment configurations for your application.

**Configurations to Generate**:

1. **Dockerfile**:
   - Multi-stage build
   - Optimized layers
   - Security best practices
   - Non-root user

2. **docker-compose.yml**:
   - Application service
   - Database
   - Redis/cache
   - Environment variables

3. **Kubernetes Manifests**:
   - Deployment
   - Service
   - Ingress
   - ConfigMap
   - Secret
   - HorizontalPodAutoscaler

4. **CI/CD Pipeline**:
   - GitHub Actions / GitLab CI
   - Build stage
   - Test stage
   - Deploy stage
   - Environment-specific configs

5. **Infrastructure as Code** (optional):
   - Terraform
   - CloudFormation
   - Pulumi

Use the `docker-optimizer`, `kubernetes-manifest-generator`, `cicd-pipeline-builder`, and `infrastructure-as-code` skills.

**Platform Support**:
- AWS (ECS, EKS, Lambda)
- Google Cloud (GKE, Cloud Run)
- Azure (AKS, Container Instances)
- DigitalOcean
- Heroku
- Vercel/Netlify (frontend)

**Output**:
- All configuration files
- README with deployment instructions
- Environment variable templates
- Secrets management guide
