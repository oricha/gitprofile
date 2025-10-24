# 🚀 GitPortfolio Deployment Guide

This guide explains how to deploy the GitPortfolio application using Docker containers to both test (Dokploy) and production (Northflank) environments.

## 📋 Prerequisites

- Docker Hub account
- GitHub repository with Actions enabled
- Dokploy instance for testing
- Northflank account for production
- Docker installed locally (for testing)

## 🏗️ Architecture Overview

```
GitHub Repository
       ↓
GitHub Actions (Build & Push)
       ↓
Docker Hub Registry
       ↓
┌─────────────────┬─────────────────┐
│   Dokploy       │   Northflank    │
│   (Testing)      │   (Production)  │
└─────────────────┴─────────────────┘
```

## 🔧 Setup Instructions

### 1. GitHub Secrets Configuration

Add the following secrets to your GitHub repository:

1. Go to your repository → Settings → Secrets and variables → Actions
2. Add these secrets:

```
DOCKER_USERNAME=your_dockerhub_username
DOCKER_PASSWORD=your_dockerhub_token
```

### 2. Docker Hub Setup

1. Create a Docker Hub account at [hub.docker.com](https://hub.docker.com)
2. Create a new repository named `gitportfolio`
3. Generate an access token:
   - Go to Account Settings → Security
   - Create a new access token
   - Use this token as `DOCKER_PASSWORD` in GitHub secrets

## 🐳 Docker Configuration

### Dockerfile Features

- **Multi-stage build** for optimized image size
- **Node.js 20 Alpine** for building
- **Nginx Alpine** for production
- **Static file serving** with proper routing
- **Security headers** and compression

### Image Details

- **Base Image**: `nginx:alpine`
- **Size**: ~25MB (optimized)
- **Port**: 80
- **Architecture**: Multi-platform (AMD64, ARM64)

## 🚀 Deployment Methods

### Method 1: Automated Deployment (Recommended)

#### For Dokploy (Testing Environment)

1. **Access Dokploy Dashboard**
   - Login to your Dokploy instance
   - Create a new application

2. **Configure Application**
   ```
   Application Name: gitportfolio-test
   Image: oricha/gitportfolio:latest
   Port: 80
   Environment: Production
   ```

3. **Environment Variables** (Optional)
   ```
   NODE_ENV=production
   BASE_URL=/gitprofile/
   ```

4. **Deploy**
   - Click "Deploy" in Dokploy
   - Monitor the deployment logs
   - Access your app at the provided URL

#### For Northflank (Production Environment)

1. **Access Northflank Dashboard**
   - Login to [app.northflank.com](https://app.northflank.com)
   - Create a new service

2. **Configure Service**
   ```
   Service Name: gitportfolio-prod
   Image: oricha/gitportfolio:latest
   Port: 80
   Replicas: 2 (for high availability)
   ```

3. **Resource Configuration**
   ```
   CPU: 0.5 cores
   Memory: 512MB
   Storage: 1GB
   ```

4. **Environment Variables**
   ```
   NODE_ENV=production
   BASE_URL=/gitprofile/
   ```

5. **Deploy**
   - Click "Deploy" in Northflank
   - Monitor the deployment status
   - Access your production app

### Method 2: Manual Deployment

#### Local Testing

```bash
# Clone the repository
git clone https://github.com/oricha/gitportfolio.git
cd gitportfolio

# Build the Docker image
docker build -t oricha/gitportfolio:latest .

# Run the container
docker run -p 8080:80 oricha/gitportfolio:latest

# Access the application
open http://localhost:8080
```

#### Manual Docker Hub Push

```bash
# Login to Docker Hub
docker login

# Tag the image
docker tag oricha/gitportfolio:latest oricha/gitportfolio:latest

# Push to Docker Hub
docker push oricha/gitportfolio:latest
```

## 🔄 CI/CD Pipeline

### GitHub Actions Workflow

The automated pipeline includes:

1. **Trigger**: Push to `main` branch
2. **Build**: Multi-platform Docker image
3. **Push**: To Docker Hub with tags
4. **Cache**: Optimized build caching

### Workflow Features

- ✅ **Multi-platform builds** (AMD64, ARM64)
- ✅ **Build caching** for faster builds
- ✅ **Automatic tagging** (latest, branch, SHA)
- ✅ **Security scanning** (if enabled)
- ✅ **Deployment summaries**

## 📊 Monitoring and Maintenance

### Health Checks

```bash
# Check container status
docker ps

# View container logs
docker logs <container_id>

# Check resource usage
docker stats <container_id>
```

### Updates

1. **Automatic Updates**: Push to `main` branch
2. **Manual Updates**: Pull latest image in deployment platform
3. **Rollback**: Use previous image tags

## 🛠️ Troubleshooting

### Common Issues

#### 1. Build Failures
```bash
# Check Docker build logs
docker build --no-cache -t oricha/gitportfolio:latest .
```

#### 2. Container Won't Start
```bash
# Check container logs
docker logs <container_id>

# Verify port mapping
docker port <container_id>
```

#### 3. Image Pull Issues
```bash
# Login to Docker Hub
docker login

# Pull latest image
docker pull oricha/gitportfolio:latest
```

### Performance Optimization

1. **Enable Gzip Compression** (already configured)
2. **Set Cache Headers** (already configured)
3. **Use CDN** for static assets
4. **Monitor Resource Usage**

## 🔒 Security Considerations

### Implemented Security Features

- ✅ **Security headers** (X-Frame-Options, X-Content-Type-Options)
- ✅ **Content Security Policy** ready
- ✅ **HTTPS enforcement** (configure in deployment platform)
- ✅ **Resource limits** (configure in deployment platform)

### Additional Security

1. **Enable HTTPS** in deployment platform
2. **Set up monitoring** and alerting
3. **Regular security updates**
4. **Backup strategies**

## 📈 Scaling

### Horizontal Scaling

- **Dokploy**: Scale replicas in dashboard
- **Northflank**: Adjust replica count in service settings
- **Load Balancing**: Automatic in both platforms

### Vertical Scaling

- **CPU**: Increase cores in deployment platform
- **Memory**: Increase RAM allocation
- **Storage**: Add persistent volumes if needed

## 📞 Support

### Getting Help

1. **GitHub Issues**: [Create an issue](https://github.com/oricha/gitportfolio/issues)
2. **Documentation**: Check this README
3. **Community**: GitHub Discussions

### Useful Commands

```bash
# Check image size
docker images oricha/gitportfolio

# Inspect image layers
docker history oricha/gitportfolio:latest

# Run interactive container
docker run -it oricha/gitportfolio:latest sh
```

## 🎯 Quick Start Checklist

- [ ] Set up GitHub secrets
- [ ] Configure Docker Hub repository
- [ ] Deploy to Dokploy (testing)
- [ ] Deploy to Northflank (production)
- [ ] Configure custom domain (optional)
- [ ] Set up monitoring (optional)
- [ ] Configure backups (optional)

---

**Happy Deploying! 🚀**

For more information, visit the [GitHub repository](https://github.com/oricha/gitportfolio).
