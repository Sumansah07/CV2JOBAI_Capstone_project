# 🚀 AI Career Portal - Setup Instructions

## 🎯 **Quick Start (Development Mode)**

### **Option 1: Frontend Only (Recommended for Demo)**
```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Start development server
npm run dev
```

**Access the application at:** http://localhost:3001

**Demo Credentials:**
- Email: `demo@example.com`
- Password: `demo123`

> ✅ **This works completely offline with mock AI data!**

---

## 🔧 **Full Stack Setup (Backend + Frontend)**

### **Prerequisites**
- Node.js 18+ installed
- MongoDB Atlas account (free) OR local MongoDB
- Google Gemini API key (free)

### **Step 1: Backend Setup**

```bash
# Navigate to backend directory
cd backend

# Install dependencies
npm install

# Configure environment variables
cp .env.example .env
```

**Edit `.env` file:**
```env
# Server Configuration
PORT=5000
NODE_ENV=development

# Database (MongoDB Atlas - Free Tier)
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/ai-career-portal

# JWT Secret (generate a random string)
JWT_SECRET=your-super-secret-jwt-key-here

# AI Services
GEMINI_API_KEY=AIzaSyBzClpbU4fkwFXQhY3AFufNfdasMPxFGUg

# File Upload
UPLOAD_PATH=./uploads
MAX_FILE_SIZE=10485760
```

**Start backend server:**
```bash
npm run dev
```

### **Step 2: Frontend Setup**

```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Configure environment variables
cp .env.example .env.local
```

**Edit `.env.local` file:**
```env
NEXT_PUBLIC_API_URL=http://localhost:5000/api
```

**Start frontend server:**
```bash
npm run dev
```

---

## 🌐 **Production Deployment**

### **Backend Deployment (Railway/Heroku)**

1. **Create Railway/Heroku app**
2. **Set environment variables:**
   - `MONGODB_URI`: Your MongoDB Atlas connection string
   - `JWT_SECRET`: Strong random string
   - `GEMINI_API_KEY`: Your Google Gemini API key
   - `NODE_ENV`: production

3. **Deploy:**
```bash
# Railway
railway deploy

# Heroku
git push heroku main
```

### **Frontend Deployment (Vercel)**

1. **Connect GitHub repository to Vercel**
2. **Set environment variables:**
   - `NEXT_PUBLIC_API_URL`: Your backend API URL

3. **Deploy automatically on push**

---

## 🤖 **AI Features Overview**

### **Google Gemini AI Integration**
- **Resume Analysis**: Comprehensive scoring and feedback
- **Job Matching**: AI-powered compatibility scoring
- **Interview Preparation**: Generate tailored questions
- **Cover Letter Generation**: Personalized cover letters
- **Career Guidance**: Skill recommendations and career paths
- **Market Analysis**: Industry insights and trends

### **Mock Data Fallback**
- Works offline for development/demo
- Realistic AI responses
- No backend required for testing

---

## 📁 **Project Structure**

```
ai-career-portal/
├── backend/                 # Node.js + Express API
│   ├── src/
│   │   ├── models/         # MongoDB schemas
│   │   ├── routes/         # API endpoints
│   │   ├── services/       # AI services (Gemini)
│   │   ├── middleware/     # Auth & validation
│   │   └── app.js         # Express app
│   ├── uploads/           # File uploads
│   └── package.json
├── frontend/               # Next.js React app
│   ├── src/
│   │   ├── app/           # Next.js 13+ app router
│   │   ├── components/    # React components
│   │   ├── contexts/      # React contexts
│   │   ├── services/      # API services
│   │   └── types/         # TypeScript types
│   └── package.json
└── README.md
```

---

## 🔑 **Key Features**

### **Authentication**
- JWT-based authentication
- Role-based access (student/recruiter)
- Secure password hashing
- Mock authentication for development

### **Resume Analysis**
- PDF/DOC file upload
- AI-powered text extraction
- Comprehensive scoring (0-100)
- ATS compatibility analysis
- Improvement suggestions

### **Job Matching**
- AI-calculated compatibility scores
- Skill gap analysis
- Personalized recommendations
- Advanced filtering

### **Dashboard Analytics**
- Profile views tracking
- Application statistics
- Match score trends
- Interactive charts

### **Modern UI/UX**
- Responsive design
- Dark/light mode support
- Loading states
- Error boundaries
- Accessibility features

---

## 🛠️ **Development Tools**

### **Backend Stack**
- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose
- **AI**: Google Gemini API
- **File Processing**: Multer, PDF-parse, Mammoth
- **Authentication**: JWT

### **Frontend Stack**
- **Framework**: Next.js 14
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Charts**: Recharts
- **Icons**: Heroicons
- **File Upload**: React Dropzone

---

## 🚨 **Troubleshooting**

### **Common Issues**

1. **401 Authentication Error**
   - Use demo credentials: `demo@example.com` / `demo123`
   - Check if backend is running
   - Verify JWT_SECRET in backend

2. **404 API Errors**
   - Ensure backend is running on port 5000
   - Check NEXT_PUBLIC_API_URL in frontend
   - Application will fallback to mock data

3. **File Upload Issues**
   - Check UPLOAD_PATH exists
   - Verify MAX_FILE_SIZE setting
   - Ensure proper file permissions

4. **AI Features Not Working**
   - Verify GEMINI_API_KEY is set
   - Check API quota limits
   - Application will use mock responses as fallback

### **Development Status Indicator**
- Green: Backend online, real AI
- Yellow: Using mock data (offline mode)
- Shows in bottom-right corner (development only)

---

## 📊 **API Endpoints**

### **Authentication**
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `GET /api/auth/me` - Get current user

### **Resume Management**
- `POST /api/resumes/upload` - Upload resume
- `GET /api/resumes` - Get user resumes
- `GET /api/resumes/:id` - Get resume analysis
- `POST /api/resumes/:id/analyze-job-match` - Job compatibility

### **Job Matching**
- `GET /api/jobs/ai-matches` - AI-powered job matches
- `POST /api/jobs/:id/generate-interview-questions` - Interview prep
- `POST /api/jobs/:id/generate-cover-letter` - Cover letter

### **Analytics**
- `GET /api/analytics/dashboard` - Dashboard stats
- `GET /api/analytics/user-stats` - User analytics

---

## 🎓 **Perfect for Academic Projects**

This AI Career Portal demonstrates:
- **Full-stack development** with modern technologies
- **AI integration** using Google Gemini API
- **Professional UI/UX** design principles
- **Scalable architecture** and best practices
- **Real-world application** solving career challenges

**Ideal for:**
- Bachelor's/Master's capstone projects
- Software engineering portfolios
- AI/ML project demonstrations
- Full-stack development showcases

---

## 📞 **Support**

For issues or questions:
1. Check the troubleshooting section
2. Review console logs for errors
3. Ensure all environment variables are set
4. Try the demo mode first (frontend only)

**The application is designed to work offline with mock data, making it perfect for demonstrations and development!**
