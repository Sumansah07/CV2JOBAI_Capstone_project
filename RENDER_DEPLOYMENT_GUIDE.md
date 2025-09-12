# 🚀 Complete Render Deployment Guide

## AI-Powered Career Portal on Render

This guide will walk you through deploying your full-stack AI Career Portal to Render platform with production-ready configuration.

---

## 📋 **Prerequisites**

### Required Accounts & Services
- ✅ **Render Account**: [Sign up at render.com](https://render.com)
- ✅ **GitHub Account**: For repository hosting
- ✅ **MongoDB Atlas**: [Free cluster at mongodb.com](https://mongodb.com)
- ✅ **Google AI Studio**: For Gemini API key
- ✅ **Domain Name** (optional): For custom domain

### Estimated Costs (Monthly)
- **Render Starter Plan**: $7/month per service × 2 = $14/month
- **MongoDB Atlas**: Free tier (512MB) or $9/month (2GB)
- **Total**: ~$14-23/month for production deployment

---

## 🗄️ **Step 1: Setup MongoDB Atlas**

### 1.1 Create MongoDB Cluster
```bash
1. Go to https://cloud.mongodb.com
2. Create new project: "AI Career Portal"
3. Build Database → Shared (Free) or Dedicated
4. Choose cloud provider & region (same as Render region for lower latency)
5. Cluster Name: "ai-career-portal-prod"
```

### 1.2 Configure Database Access
```bash
1. Database Access → Add New Database User
   - Username: aicareerportal
   - Password: Generate secure password
   - Role: Atlas Admin

2. Network Access → Add IP Address
   - Add: 0.0.0.0/0 (Allow access from anywhere)
   - Or add Render's IP ranges for better security
```

### 1.3 Get Connection String
```bash
1. Clusters → Connect → Connect your application
2. Copy connection string:
   mongodb+srv://aicareerportal:<password>@ai-career-portal-prod.xxxxx.mongodb.net/?retryWrites=true&w=majority
3. Replace <password> with actual password
4. Add database name: /ai-career-portal before ?retryWrites
```

---

## 🔑 **Step 2: Prepare API Keys**

### 2.1 Google Gemini API Key
```bash
1. Go to https://aistudio.google.com/app/apikey
2. Create API key
3. Copy the key (starts with "AIza...")
4. Enable Generative Language API in Google Cloud Console
```

### 2.2 JWT Secret Generation
```bash
# Generate a secure 256-bit secret
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

---

## 🚀 **Step 3: Deploy Backend to Render**

### 3.1 Create Web Service for Backend
```bash
1. Go to Render Dashboard
2. New → Web Service
3. Connect GitHub repository
4. Configure service:
   - Name: ai-career-portal-backend
   - Region: Oregon (or closest to your users)
   - Branch: main
   - Root Directory: backend
   - Runtime: Node
   - Build Command: npm ci --only=production && node src/utils/db-init.js
   - Start Command: npm start
   - Plan: Starter ($7/month)
```

### 3.2 Set Environment Variables
Copy these to Render Dashboard → Environment Variables:

```bash
# Core Configuration
NODE_ENV=production
PORT=10000
HOST=0.0.0.0

# Database
MONGODB_URI=mongodb+srv://aicareerportal:YOUR_PASSWORD@ai-career-portal-prod.xxxxx.mongodb.net/ai-career-portal?retryWrites=true&w=majority

# JWT (Replace with your generated secret)
JWT_SECRET=your-32-character-hex-secret-from-step-2-2
JWT_EXPIRE=24h

# Frontend URL (Will update after frontend deployment)
FRONTEND_URL=https://your-frontend-app-name.onrender.com

# File Upload
MAX_FILE_SIZE=5242880
UPLOAD_PATH=/tmp/uploads
ALLOWED_FILE_TYPES=pdf,doc,docx

# AI Services
GEMINI_API_KEY=your-gemini-api-key-from-step-2-1

# Security
BCRYPT_ROUNDS=12
TRUST_PROXY=true

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=50

# Logging
LOG_LEVEL=info

# Database Pool
DB_MIN_POOL_SIZE=2
DB_MAX_POOL_SIZE=10
DB_CONNECTION_TIMEOUT=30000
```

### 3.3 Deploy Backend
```bash
1. Click "Create Web Service"
2. Wait for build to complete (~5-10 minutes)
3. Note the backend URL: https://your-backend-name.onrender.com
```

---

## 🌐 **Step 4: Deploy Frontend to Render**

### 4.1 Create Web Service for Frontend
```bash
1. Render Dashboard → New → Web Service
2. Same GitHub repository
3. Configure service:
   - Name: ai-career-portal-frontend
   - Region: Same as backend
   - Branch: main
   - Root Directory: frontend
   - Runtime: Node
   - Build Command: npm ci && npm run build
   - Start Command: npm start
   - Plan: Starter ($7/month)
```

### 4.2 Set Frontend Environment Variables
```bash
# Environment
NODE_ENV=production

# API Configuration (Use your actual backend URL from Step 3.3)
NEXT_PUBLIC_API_URL=https://your-backend-name.onrender.com/api
NEXT_PUBLIC_APP_URL=https://your-frontend-name.onrender.com

# Features
NEXT_PUBLIC_ENABLE_AI_FEATURES=true
NEXT_PUBLIC_ENABLE_MOCK_DATA=false
NEXT_PUBLIC_ENABLE_DEV_STATUS=false
NEXT_PUBLIC_ENABLE_DEBUG_MODE=false

# File Upload
NEXT_PUBLIC_MAX_FILE_SIZE=5242880
NEXT_PUBLIC_SUPPORTED_FORMATS=pdf,doc,docx

# Performance
NEXT_PUBLIC_ENABLE_PWA=true
NEXT_PUBLIC_CACHE_CONTROL=public, max-age=31536000
```

### 4.3 Deploy Frontend
```bash
1. Click "Create Web Service"
2. Wait for build to complete (~3-5 minutes)
3. Note the frontend URL: https://your-frontend-name.onrender.com
```

---

## 🔄 **Step 5: Update Backend with Frontend URL**

### 5.1 Update Backend Environment
```bash
1. Go to Backend service in Render Dashboard
2. Environment → Edit FRONTEND_URL
3. Set: https://your-frontend-name.onrender.com
4. Save changes (triggers redeploy)
```

---

## ✅ **Step 6: Verify Deployment**

### 6.1 Backend Health Check
```bash
# Test these endpoints:
https://your-backend-name.onrender.com/api/health
https://your-backend-name.onrender.com/api/monitoring/ready

# Should return 200 OK with service information
```

### 6.2 Frontend Access Test
```bash
# Open in browser:
https://your-frontend-name.onrender.com

# Should load the homepage successfully
```

### 6.3 Full Integration Test
```bash
1. Go to frontend URL
2. Click "Register" 
3. Create a test account
4. Try to log in
5. Upload a test resume
6. Check if AI analysis works
```

---

## 🔧 **Step 7: Configuration & Optimization**

### 7.1 Custom Domain (Optional)
```bash
1. Render Dashboard → Frontend Service
2. Settings → Custom Domains
3. Add your domain: yourapp.com
4. Configure DNS:
   - Type: CNAME
   - Name: www (or @)
   - Value: your-frontend-name.onrender.com
```

### 7.2 SSL Configuration
```bash
# Render automatically provides SSL certificates
# No additional configuration needed
# Your app will be available at https://
```

### 7.3 Performance Optimization
```bash
1. Enable Auto-Deploy for continuous deployment
2. Set up monitoring alerts in Render Dashboard
3. Consider upgrading to Standard plan for:
   - Always-on instances (no cold starts)
   - More memory and CPU
   - Priority support
```

---

## 📊 **Step 8: Monitoring & Maintenance**

### 8.1 Built-in Monitoring
```bash
# Render provides:
- Service metrics (CPU, Memory, Response time)
- Logs (real-time and historical)
- Deploy history
- Uptime monitoring
```

### 8.2 Custom Monitoring Endpoints
```bash
# Your app includes these monitoring endpoints:
GET /api/health              # Overall health status
GET /api/monitoring/metrics  # Application metrics  
GET /api/monitoring/ready    # Readiness check
GET /api/monitoring/live     # Liveness probe
```

### 8.3 Log Access
```bash
# View logs in Render Dashboard:
1. Service → Logs tab
2. Real-time logs
3. Historical logs (48 hours retention)
4. Download logs for analysis
```

---

## 🚨 **Troubleshooting**

### Common Issues & Solutions

#### **Backend Build Fails**
```bash
# Check logs for:
1. Missing environment variables
2. Database connection issues
3. Dependency installation errors

# Solution:
1. Verify all environment variables are set
2. Check MongoDB Atlas whitelist
3. Ensure package.json is valid
```

#### **Frontend Build Fails**
```bash
# Common causes:
1. API URL misconfiguration
2. Environment variable issues
3. Build timeout

# Solution:
1. Check NEXT_PUBLIC_API_URL is correct
2. Verify environment variables
3. Contact Render support for build timeout
```

#### **Database Connection Issues**
```bash
# Check:
1. MongoDB Atlas connection string
2. Network access configuration
3. Database user permissions

# Test connection:
node -e "require('mongoose').connect('YOUR_MONGODB_URI').then(() => console.log('Connected!'))"
```

#### **CORS Issues**
```bash
# If frontend can't connect to backend:
1. Verify FRONTEND_URL in backend env vars
2. Check browser console for CORS errors
3. Ensure both services are deployed and running
```

### **Service Won't Start**
```bash
# Check Render logs for:
1. Port binding issues (should use PORT env var)
2. Missing critical dependencies
3. Database initialization failures

# Your app automatically handles Render's PORT environment variable
```

---

## 🔄 **Continuous Deployment**

### Automatic Deploys
```bash
# Render automatically deploys when you push to main branch
1. Make changes to your code
2. Git push to main branch
3. Render detects changes and redeploys
4. Zero-downtime deployment
```

### Manual Deploy
```bash
# From Render Dashboard:
1. Go to service
2. Click "Manual Deploy"
3. Select branch
4. Deploy
```

---

## 💰 **Cost Optimization**

### Free Tier Limitations
```bash
# Render Free tier:
- Services spin down after 15 minutes of inactivity
- 750 hours/month (enough for one always-on service)
- 500MB RAM, 0.1 CPU

# For production, upgrade to Starter ($7/month):
- Always-on (no spin down)
- 512MB RAM, 0.5 CPU
- Better performance
```

### Scaling Options
```bash
# Render plans:
- Starter: $7/month (512MB RAM, 0.5 CPU)
- Standard: $25/month (2GB RAM, 1 CPU)
- Pro: $85/month (4GB RAM, 2 CPU)
- Pro Plus: $340/month (8GB RAM, 4 CPU)
```

---

## 📚 **Additional Resources**

### Render Documentation
- [Render Node.js Guide](https://render.com/docs/node-express-app)
- [Environment Variables](https://render.com/docs/environment-variables)
- [Custom Domains](https://render.com/docs/custom-domains)

### Project Resources
- **Project Description**: *"Full-stack AI-powered career portal with intelligent resume analysis, job matching, and placement analytics using Node.js, Next.js, MongoDB, and Google Gemini API deployed on Render."*
- **GitHub Repository**: Your project repository
- **Live Demo**: https://your-frontend-name.onrender.com

---

## ✅ **Deployment Checklist**

### Pre-Deployment
- [ ] MongoDB Atlas cluster created and configured
- [ ] API keys obtained (Gemini, JWT secret)
- [ ] GitHub repository ready
- [ ] Environment variables prepared

### Deployment
- [ ] Backend service created and deployed
- [ ] Frontend service created and deployed
- [ ] Environment variables configured
- [ ] Cross-service URLs updated

### Post-Deployment
- [ ] Health checks passing
- [ ] Full application flow tested
- [ ] Custom domain configured (if applicable)
- [ ] Monitoring set up
- [ ] Documentation updated

---

## 🎉 **Success!**

Your **AI-Powered Career Portal** is now live on Render! 

🌐 **Frontend URL**: https://your-frontend-name.onrender.com  
🔗 **Backend API**: https://your-backend-name.onrender.com/api  
📊 **Health Check**: https://your-backend-name.onrender.com/api/health

### What You've Achieved:
- ✅ **Production deployment** on reliable cloud infrastructure
- ✅ **Automatic scaling** and zero-downtime deployments  
- ✅ **SSL encryption** and security best practices
- ✅ **Real-time monitoring** and logging
- ✅ **Professional portfolio piece** ready for showcase

---

*Your AI Career Portal is now serving users globally with enterprise-grade reliability and performance!* 🚀