# Multi-Container Fibonacci Calculator

A deliberately over-engineered Fibonacci sequence calculator built with a microservices architecture to demonstrate multi-container Docker deployment on AWS Elastic Beanstalk.

## 🎯 Purpose

This application calculates Fibonacci sequence values at given indices. While the functionality is simple, the architecture is intentionally complex to showcase:
- Multi-container Docker applications
- Microservices communication
- CI/CD pipeline with GitHub Actions
- AWS Elastic Beanstalk deployment
- Container orchestration with Docker Compose

## 🏗️ Architecture

![ProjectArchitecture](/home/szidzse/Pictures/Screenshots)

The application consists of multiple services:

### Frontend (React Client)
- **Location**: `./client`
- **Purpose**: User interface for entering Fibonacci indices and displaying results
- **Technology**: React.js with Nginx serving static files

### Backend API (Express Server)
- **Location**: `./server`
- **Purpose**: RESTful API handling requests and database operations
- **Technology**: Node.js with Express
- **Database**: PostgreSQL (AWS RDS)

### Worker Service
- **Location**: `./worker`
- **Purpose**: Background processing of Fibonacci calculations
- **Technology**: Node.js
- **Cache**: Redis (AWS ElastiCache)

### Reverse Proxy (Nginx)
- **Location**: `./nginx`
- **Purpose**: Routes requests between client and server
- **Configuration**: Custom nginx configuration

## 🚀 Deployment

### AWS Services Used
- **AWS Elastic Beanstalk**: Application hosting and orchestration
- **AWS RDS (PostgreSQL)**: Relational database for storing calculation history
- **AWS ElastiCache (Redis)**: Caching layer for computed Fibonacci values

### CI/CD Pipeline

The deployment is automated using GitHub Actions:

1. **Trigger**: Push to `main` branch
2. **Build**: Creates Docker images for all services
3. **Test**: Runs automated tests on the React application
4. **Push**: Uploads images to Docker Hub
5. **Deploy**: Packages and deploys to AWS Elastic Beanstalk

### Environment Configuration

- **Production**: `docker-compose.yml`
- **Development**: `docker-compose-dev.yml`

### Project Structure

```
.
├── client/                 # React frontend
│   ├── src/
│   │   ├── App.js         # Main application component
│   │   ├── Fib.js         # Fibonacci calculator component
│   │   └── OtherPage.js   # Additional page component
│   ├── nginx/             # Nginx configuration for production
│   └── Dockerfile*        # Container definitions
├── server/                # Express.js backend API
│   ├── index.js          # Main server file
│   ├── keys.js           # Environment configuration
│   └── Dockerfile*       # Container definitions
├── worker/               # Background worker service
│   ├── index.js         # Worker process
│   ├── keys.js          # Environment configuration
│   └── Dockerfile*      # Container definitions
├── nginx/               # Reverse proxy configuration
│   ├── default.conf     # Nginx routing rules
│   └── Dockerfile*      # Container definitions
├── docker-compose.yml          # Production configuration
├── docker-compose-dev.yml      # Development configuration
└── .github/workflows/deploy.yml # CI/CD pipeline
```

## 🔧 Configuration

### Environment Variables

The application uses the following environment variables:

- `REDIS_HOST`: Redis server hostname
- `REDIS_PORT`: Redis server port
- `PGUSER`: PostgreSQL username
- `PGHOST`: PostgreSQL hostname
- `PGDATABASE`: PostgreSQL database name
- `PGPASSWORD`: PostgreSQL password
- `PGPORT`: PostgreSQL port

### Docker Images

The application builds and uses the following Docker images:
- `szidzse/multi-client-up`: React frontend
- `szidzse/multi-nginx-up`: Nginx reverse proxy
- `szidzse/multi-server-up`: Express.js backend
- `szidzse/multi-worker-up`: Background worker

## 📊 How It Works

1. User enters a Fibonacci index in the React frontend
2. Request is routed through Nginx to the Express server
3. Server checks PostgreSQL for previously calculated values
4. If not cached, server queues calculation request in Redis
5. Worker service picks up the request and calculates the Fibonacci value
6. Result is stored in Redis cache and PostgreSQL database
7. Frontend displays the calculated Fibonacci value

## 🚦 Deployment Status

The application is automatically deployed to AWS Elastic Beanstalk when changes are pushed to the `main` branch. The deployment pipeline includes:

- Automated testing
- Multi-container image building
- Docker Hub registry updates
- AWS Elastic Beanstalk deployment

## 📝 Notes

This is an educational project designed to demonstrate enterprise-level container orchestration patterns. The Fibonacci calculation could be implemented much more simply, but the complex architecture serves as a learning tool for:

- Microservices architecture patterns
- Docker multi-container applications
- Cloud deployment strategies
- CI/CD pipeline implementation
- Database and caching integration
