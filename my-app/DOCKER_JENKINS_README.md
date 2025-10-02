# Docker & Jenkins Setup

## Docker Files Created

### 1. Dockerfile
- **Multi-stage build**: Optimized for production
- **Build stage**: Uses Node.js 20 to install dependencies and build the app
- **Production stage**: Uses Nginx Alpine to serve the built application
- **Health check**: Includes health monitoring

### 2. .dockerignore
- Excludes unnecessary files from Docker build context
- Reduces image size and build time

### 3. docker-compose.yml
- **Development profile**: For local development with hot reload
- **Production profile**: For production deployment
- **Optional Nginx**: Reverse proxy configuration

### 4. nginx.conf
- Optimized Nginx configuration for React SPA
- Includes gzip compression, security headers, and caching
- Handles client-side routing

## Usage Instructions

### Docker Commands

#### Build and run for production:
```bash
# Build the Docker image
docker build -t my-react-app .

# Run the container
docker run -p 80:80 my-react-app
```

#### Using Docker Compose:

**For development:**
```bash
# Start development environment with hot reload
docker-compose --profile dev up

# Access the app at http://localhost:5173
```

**For production:**
```bash
# Start production environment
docker-compose --profile prod up -d

# Access the app at http://localhost:80
```

**With Nginx reverse proxy:**
```bash
# Start with Nginx proxy
docker-compose --profile nginx up -d

# Access the app at http://localhost:8080
```

### Jenkins Setup

#### Prerequisites:
1. Jenkins server with Docker support
2. Node.js plugin installed
3. Docker plugin installed
4. Credentials configured for Docker registry

#### Pipeline Features:
- **Multi-stage pipeline**: Checkout, Install, Lint, Build, Test, Docker Build, Security Scan, Deploy
- **Branch-based deployment**: Only deploys from main branch
- **Manual approval**: Production deployment requires manual approval
- **Environment variables**: Configurable registry and image names
- **Security scanning**: Placeholder for security tools (Trivy, etc.)

#### Configuration Steps:

1. **Install required Jenkins plugins:**
   - Pipeline
   - Docker Pipeline
   - NodeJS

2. **Configure Node.js in Jenkins:**
   - Go to Manage Jenkins > Global Tool Configuration
   - Add Node.js installation (version 20)

3. **Add Docker registry credentials:**
   - Go to Manage Jenkins > Credentials
   - Add Docker registry credentials with ID: `docker-registry-credentials`

4. **Create Jenkins Pipeline:**
   - Create new Pipeline job
   - Point to your repository
   - Jenkins will automatically detect the Jenkinsfile

5. **Customize the Jenkinsfile:**
   - Update `DOCKER_REGISTRY` environment variable
   - Add your test commands in the Test stage
   - Configure deployment scripts for your infrastructure

#### Pipeline Parameters:
- `DEPLOY_TO_PRODUCTION`: Boolean parameter to control production deployment

## Environment Variables

You can customize the following variables in the Jenkinsfile:
- `NODE_VERSION`: Node.js version (default: 20)
- `APP_NAME`: Application name
- `DOCKER_REGISTRY`: Your Docker registry URL

## Security Considerations

1. **Docker Image Security:**
   - Uses Alpine Linux for smaller attack surface
   - Runs as non-root user in production
   - Includes health checks

2. **Nginx Security:**
   - Security headers configured
   - Content Security Policy
   - XSS protection

3. **Jenkins Security:**
   - Credentials stored securely
   - Branch-based deployment restrictions
   - Manual approval for production

## Monitoring and Logging

- Health check endpoint: `/health`
- Container logs available through Docker
- Jenkins build logs and artifacts

## Next Steps

1. Configure your Docker registry
2. Set up Jenkins server
3. Add test scripts to package.json
4. Configure deployment targets
5. Set up monitoring and alerting