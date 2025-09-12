# 🚀 Production Deployment Guide

## Prerequisites

### System Requirements
- **Docker & Docker Compose**: Latest stable version
- **Node.js**: v18+ (for local development)
- **Git**: For code deployment
- **SSL Certificates**: For HTTPS (Let's Encrypt recommended)

### External Services
- **MongoDB Atlas** or self-hosted MongoDB cluster
- **Redis** instance for caching and sessions
- **Gemini API** key from Google AI Studio
- **Sentry** account for error tracking (optional but recommended)
- **Domain name** and DNS configuration

---

## 🔧 Initial Setup

### 1. Clone and Prepare Repository
```bash
git clone <your-repository-url>
cd ai-career-portal
chmod +x scripts/*.sh
./scripts/setup-production.sh
```

### 2. Configure Environment Variables

#### Backend (.env)
```bash
cp backend/.env.production backend/.env
# Edit backend/.env with your production values
```

**Critical variables to update:**
- `MONGODB_URI`: Your production MongoDB connection string
- `JWT_SECRET`: Strong 256-bit secret key
- `GEMINI_API_KEY`: Your Google AI API key
- `FRONTEND_URL`: Your production frontend domain
- `SENTRY_DSN`: Your Sentry project DSN

#### Frontend (.env.local)
```bash
cp frontend/.env.production frontend/.env.local
# Edit frontend/.env.local with your production values
```

**Critical variables to update:**
- `NEXT_PUBLIC_API_URL`: Your production backend API URL
- `NEXT_PUBLIC_APP_URL`: Your production frontend URL

### 3. SSL Certificate Setup

For production HTTPS:
```bash
# Option 1: Let's Encrypt (recommended)
certbot certonly --standalone -d your-domain.com
cp /etc/letsencrypt/live/your-domain.com/fullchain.pem nginx/ssl/cert.pem
cp /etc/letsencrypt/live/your-domain.com/privkey.pem nginx/ssl/key.pem

# Option 2: Use existing certificates
cp your-certificate.pem nginx/ssl/cert.pem
cp your-private-key.pem nginx/ssl/key.pem
```

---

## 🐳 Docker Deployment

### 1. Validate Before Deployment
```bash
./scripts/validate-production.sh
```

### 2. Deploy to Production
```bash
./scripts/deploy-production.sh
```

### 3. Monitor Deployment
```bash
# View all services
docker-compose ps

# View logs
docker-compose logs -f

# View specific service logs
docker-compose logs -f backend
docker-compose logs -f frontend
```

---

## 🌐 Manual Deployment (Without Docker)

### 1. Setup Production Database
```bash
# MongoDB setup with authentication
mongosh
use ai-career-portal
db.createUser({
  user: "app_user",
  pwd: "secure_password",
  roles: [{ role: "readWrite", db: "ai-career-portal" }]
})
```

### 2. Backend Deployment
```bash
cd backend
npm ci --only=production
npm run build
pm2 start src/app.js --name "ai-career-portal-backend" --env production
```

### 3. Frontend Deployment
```bash
cd frontend
npm ci --only=production
npm run build
pm2 start npm --name "ai-career-portal-frontend" -- start
```

### 4. Nginx Configuration
```bash
# Copy nginx configuration
sudo cp nginx/nginx.conf /etc/nginx/sites-available/ai-career-portal
sudo ln -s /etc/nginx/sites-available/ai-career-portal /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

---

## 📊 Monitoring and Maintenance

### Health Checks
- **Backend Health**: `https://your-domain.com/api/health`
- **Monitoring Metrics**: `https://your-domain.com/api/monitoring/metrics`
- **Database Status**: `https://your-domain.com/api/monitoring/ready`

### Log Files (Docker)
```bash
# Application logs
docker-compose exec backend ls -la /app/logs/

# View real-time logs
docker-compose logs -f --tail=100 backend
```

### Performance Monitoring
```bash
# Database performance
docker-compose exec mongodb mongo --eval "db.stats()"

# Container resource usage
docker stats
```

---

## 🔒 Security Checklist

### Before Going Live
- [ ] Update all default passwords
- [ ] Configure firewall rules
- [ ] Enable HTTPS with valid certificates
- [ ] Set up backup procedures
- [ ] Configure monitoring and alerting
- [ ] Review and test disaster recovery procedures
- [ ] Perform security audit
- [ ] Set up rate limiting
- [ ] Configure CORS properly
- [ ] Enable security headers

### Environment Security
- [ ] Secure environment variable storage
- [ ] Rotate API keys regularly
- [ ] Monitor for security vulnerabilities
- [ ] Keep dependencies updated
- [ ] Regular security audits

---

## 🚨 Troubleshooting

### Common Issues

#### Backend Won't Start
```bash
# Check logs
docker-compose logs backend

# Common causes:
# 1. Database connection issues
# 2. Missing environment variables
# 3. Port conflicts
```

#### Frontend Build Fails
```bash
# Check environment variables
cat frontend/.env.local

# Rebuild with detailed logs
docker-compose build --no-cache frontend
```

#### Database Connection Issues
```bash
# Test MongoDB connectivity
docker-compose exec backend node src/utils/db-check.js

# Check MongoDB logs
docker-compose logs mongodb
```

### Performance Issues
```bash
# Monitor resource usage
docker stats

# Check slow queries
docker-compose exec mongodb mongo --eval "db.setProfilingLevel(2)"

# Analyze logs for bottlenecks
docker-compose logs backend | grep "duration"
```

---

## 📈 Scaling Considerations

### Horizontal Scaling
- Use load balancer (Nginx/HAProxy)
- Implement Redis for session storage
- Consider database read replicas
- Use CDN for static assets

### Vertical Scaling
- Monitor resource usage
- Optimize database queries
- Implement caching strategies
- Use connection pooling

---

## 🔄 Maintenance

### Regular Tasks
- **Daily**: Monitor logs and error rates
- **Weekly**: Update dependencies, security patches
- **Monthly**: Performance review, backup verification
- **Quarterly**: Security audit, disaster recovery testing

### Backup Procedures
```bash
# Database backup
docker-compose exec mongodb mongodump --out /data/backup/$(date +%Y%m%d)

# Full system backup
tar -czf backup-$(date +%Y%m%d).tar.gz \
  backend/.env \
  frontend/.env.local \
  nginx/ssl/ \
  data/
```

---

## 📞 Support

### Monitoring Endpoints
- **Health Check**: `/api/health`
- **Readiness**: `/api/monitoring/ready`
- **Liveness**: `/api/monitoring/live`
- **Metrics**: `/api/monitoring/metrics`

### Emergency Procedures
1. **Service Down**: Restart containers
2. **Database Issues**: Check connection and disk space
3. **High Load**: Scale services horizontally
4. **Security Incident**: Review logs, update credentials

---

## ✅ Post-Deployment Checklist

- [ ] All services running and responding
- [ ] SSL certificates installed and working
- [ ] Database initialized with proper indexes
- [ ] Monitoring and logging configured
- [ ] Backup procedures tested
- [ ] Performance baselines established
- [ ] Security scan completed
- [ ] User acceptance testing completed
- [ ] Documentation updated
- [ ] Team trained on production procedures

---

## 🎯 Success Metrics

Monitor these KPIs post-deployment:
- **Uptime**: Target 99.9%
- **Response Time**: API <500ms, Pages <2s
- **Error Rate**: <1%
- **User Satisfaction**: Based on feedback
- **Security**: Zero critical vulnerabilities

---

*This guide provides a comprehensive framework for deploying and maintaining the AI Career Portal in production. Adjust configurations based on your specific infrastructure and requirements.*