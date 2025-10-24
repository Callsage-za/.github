# Call Center AI System - Master Documentation

> **Complete System Setup Guide** - This is the master documentation for the full Call Center AI system, including both frontend and backend components.

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    Call Center AI System                        │
├─────────────────────────────────────────────────────────────────┤
│  Frontend (Next.js)          │  Backend (NestJS)              │
│  Port: 4001                  │  Port: 8787                    │
│  ┌─────────────────────────┐ │  ┌─────────────────────────────┐ │
│  │ • Real-time Chat UI     │ │  │ • WebSocket Gateway         │ │
│  │ • Document Management   │ │  │ • AI Chat Processing        │ │
│  │ • Call Analytics        │ │  │ • Audio Processing          │ │
│  │ • File Upload           │ │  │ • Document Processing      │ │
│  │ • JIRA Integration      │ │  │ • Policy Management         │ │
│  │ • Authentication        │ │  │ • Database Operations       │ │
│  └─────────────────────────┘ │  └─────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
│  External Services                                               │
│  ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐    │
│  │ Google Gemini   │ │ Google Cloud    │ │ MySQL Database  │    │
│  │ AI API          │ │ Speech API      │ │                 │    │
│  └─────────────────┘ └─────────────────┘ └─────────────────┘    │
└─────────────────────────────────────────────────────────────────┘
```

## 🚀 Complete System Setup

### Prerequisites
- Node.js (v18 or higher)
- Yarn package manager
- MySQL database
- Google Cloud account with APIs enabled
- Git

### Required API Keys & Services

#### 1. Google Cloud Services
- **Google Gemini AI API**: For chat and document processing
- **Google Cloud Speech API**: For audio transcription
- **Service Account**: For authentication

#### 2. Database
- **MySQL**: Primary database for all data storage

#### 3. Optional Services
- **JIRA**: For ticket management (if using JIRA integration)

## 📋 Step-by-Step Setup

### Step 1: Clone Both Repositories

```bash
# Clone backend repository
git clone <backend-repo-url>
cd ai-copilot

# Clone frontend repository (in separate directory)
git clone <frontend-repo-url>
cd callcenter-ui
```

### Step 2: Backend Setup (Start Here)

#### 2.1 Install Dependencies
```bash
cd ai-copilot
yarn install
```

#### 2.2 Environment Configuration
Create `.env` file in the backend directory:

```env
# Database Configuration
DB_HOST=localhost
DB_PORT=3306
DB_USERNAME=your_mysql_username
DB_PASSWORD=your_mysql_password
DB_DATABASE=ai_pilot

# JWT Authentication
JWT_SECRET=your_super_secret_jwt_key_here_make_it_long_and_random
JWT_EXPIRES_IN=24h

# Google Cloud Configuration
GOOGLE_APPLICATION_CREDENTIALS=/path/to/your/service-account-key.json
GEMINI_API_KEY=your_gemini_api_key_here

# Application Configuration
NODE_ENV=development
PORT=8787

# CORS Configuration (Frontend URLs)
ALLOWED_ORIGINS=http://localhost:4001,http://localhost:3000,https://your-production-domain.com
```

#### 2.3 Database Setup
```bash
# Generate database migrations
yarn generate-migration

# Run migrations to create tables
yarn migrate-db
```

#### 2.4 Start Backend Server
```bash
# Start development server
yarn start:dev
```

**Backend should now be running at `http://localhost:8787`**

### Step 3: Frontend Setup

#### 3.1 Install Dependencies
```bash
cd callcenter-ui
yarn install
```

#### 3.2 Environment Configuration
Create `.env.local` file in the frontend directory:

```env
# Backend API Configuration
NEXT_PUBLIC_API_URL=http://localhost:8787
NEXT_PUBLIC_SOCKET_URL=http://localhost:8787

# Application Configuration
NEXT_PUBLIC_APP_NAME=Call Center AI
NEXT_PUBLIC_APP_VERSION=1.0.0
NEXT_PUBLIC_DEBUG=false
```

#### 3.3 Start Frontend Server
```bash
# Start development server
yarn dev
```

**Frontend should now be running at `http://localhost:4001`**

## 🔑 Required API Keys Setup

### Google Cloud Setup

#### 1. Create Google Cloud Project
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing one
3. Enable billing (required for API usage)

#### 2. Enable Required APIs
```bash
# Enable Gemini AI API
gcloud services enable generativelanguage.googleapis.com

# Enable Speech-to-Text API
gcloud services enable speech.googleapis.com
```

#### 3. Create Service Account
1. Go to IAM & Admin > Service Accounts
2. Create new service account
3. Download JSON key file
4. Set path in `GOOGLE_APPLICATION_CREDENTIALS`

#### 4. Get Gemini API Key
1. Go to AI Studio (https://aistudio.google.com/)
2. Create API key
3. Add to backend `.env` as `GEMINI_API_KEY`

### MySQL Database Setup

#### 1. Install MySQL
```bash
# macOS with Homebrew
brew install mysql
brew services start mysql

# Ubuntu/Debian
sudo apt-get install mysql-server
sudo systemctl start mysql

# Windows
# Download from https://dev.mysql.com/downloads/mysql/
```

#### 2. Create Database
```sql
CREATE DATABASE ai_pilot;
CREATE USER 'ai_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON ai_pilot.* TO 'ai_user'@'localhost';
FLUSH PRIVILEGES;
```

## 🔌 System Integration

### API Endpoints

#### Authentication
- `POST /auth/login` - User login
- `POST /auth/register` - User registration
- `GET /auth/profile` - Get user profile

#### Chat & Real-time
- `POST /chat/message` - Send chat message
- `GET /chat/conversations` - Get user conversations
- `GET /chat/conversations/:id` - Get specific conversation
- **WebSocket**: `/socket.io` - Real-time chat communication

#### File Management
- `POST /file-upload` - Upload files
- `GET /uploads/*` - Serve uploaded files
- `DELETE /file-upload/:id` - Delete file

#### Audio Processing
- `POST /speech/process` - Process audio files
- `GET /speech/audio/:id` - Get audio analysis results
- `GET /speech/audio` - List audio files

#### Document Management
- `GET /documents` - List documents
- `POST /documents` - Upload document
- `GET /documents/:id` - Get document details
- `DELETE /documents/:id` - Delete document

#### Call Analytics
- `GET /calls` - List calls
- `GET /calls/:id` - Get call details
- `POST /calls/search` - Search calls
- `GET /calls/analytics` - Get call statistics

#### Policy Management
- `GET /policies` - List policies
- `POST /policies` - Create policy
- `GET /policies/:id` - Get policy details
- `POST /policies/:id/audit` - Audit policy compliance

### WebSocket Events

#### Client to Server
- `send-message` - Send chat message
- `join-conversation` - Join conversation room
- `leave-conversation` - Leave conversation room
- `typing-start` - User started typing
- `typing-stop` - User stopped typing

#### Server to Client
- `new-message` - New message received
- `user-typing` - User is typing
- `user-stopped-typing` - User stopped typing
- `conversation-updated` - Conversation updated
- `file-uploaded` - File upload completed
- `audio-processed` - Audio processing completed

## 🗄️ Database Schema

### Core Tables
- **users** - User accounts and authentication
- **conversations** - Chat conversations
- **messages** - Individual chat messages
- **calls** - Call recordings and analytics
- **audio_files** - Audio processing results
- **documents** - Document management
- **policies** - Policy documents
- **file_uploads** - File upload tracking
- **memory_facts** - Conversation memory

### Key Relationships
```
users (1) ──→ (many) conversations
conversations (1) ──→ (many) messages
conversations (1) ──→ (many) memory_facts
calls (1) ──→ (many) audio_files
documents (1) ──→ (many) policies
```

## 🔧 Development Workflow

### Backend Development
```bash
cd ai-copilot

# Start with file watching
yarn start:dev

# Start with debugging
yarn start:debug

# Run tests
yarn test

# Generate new migration
yarn generate-migration

# Run migrations
yarn migrate-db
```

### Frontend Development
```bash
cd callcenter-ui

# Start with Turbopack
yarn dev

# Build for production
yarn build

# Start production server
yarn start
```

### Full System Development
```bash
# Terminal 1: Start Backend
cd ai-copilot
yarn start:dev

# Terminal 2: Start Frontend
cd callcenter-ui
yarn dev
```

## 🚀 Production Deployment

### Backend Production
```bash
# Build backend
cd ai-copilot
yarn build

# Set production environment
export NODE_ENV=production
export DB_HOST=your_production_db_host
export DB_PASSWORD=your_production_password
export JWT_SECRET=your_production_jwt_secret
export GOOGLE_APPLICATION_CREDENTIALS=/path/to/production/credentials.json

# Start production server
yarn start:prod
```

### Frontend Production
```bash
# Build frontend
cd callcenter-ui
yarn build

# Set production environment
export NEXT_PUBLIC_API_URL=https://your-api-domain.com
export NEXT_PUBLIC_SOCKET_URL=https://your-api-domain.com

# Start production server
yarn start
```

### Docker Deployment
```yaml
# docker-compose.yml
version: '3.8'
services:
  backend:
    build: ./ai-copilot
    ports:
      - "8787:8787"
    environment:
      - NODE_ENV=production
      - DB_HOST=mysql
      - DB_PASSWORD=your_password
    depends_on:
      - mysql

  frontend:
    build: ./callcenter-ui
    ports:
      - "4001:4001"
    environment:
      - NEXT_PUBLIC_API_URL=http://backend:8787
      - NEXT_PUBLIC_SOCKET_URL=http://backend:8787
    depends_on:
      - backend

  mysql:
    image: mysql:8.0
    environment:
      - MYSQL_ROOT_PASSWORD=your_password
      - MYSQL_DATABASE=ai_pilot
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

## 🔒 Security Configuration

### Backend Security
- JWT authentication with secure secrets
- CORS configuration for allowed origins
- Input validation with class-validator
- File upload security with type checking
- SQL injection protection with TypeORM

### Frontend Security
- Protected routes with authentication
- Secure API communication
- Input sanitization
- XSS protection

## 📊 Monitoring & Logging

### Backend Monitoring
- Request/response logging
- Error tracking and reporting
- Database query performance
- WebSocket connection monitoring
- API usage analytics

### Frontend Monitoring
- User interaction tracking
- Performance metrics
- Error boundary logging
- Real-time connection status

## 🆘 Troubleshooting

### Common Issues

#### Backend Issues
1. **Database Connection Failed**
   - Check MySQL credentials
   - Verify database exists
   - Check network connectivity

2. **Google Cloud Authentication Failed**
   - Verify service account key path
   - Check API permissions
   - Ensure billing is enabled

3. **CORS Errors**
   - Add frontend URL to `ALLOWED_ORIGINS`
   - Check frontend is running on correct port

#### Frontend Issues
1. **Backend Connection Failed**
   - Verify backend is running on port 8787
   - Check `NEXT_PUBLIC_API_URL` in `.env.local`
   - Check browser console for errors

2. **WebSocket Connection Failed**
   - Verify backend WebSocket is enabled
   - Check `NEXT_PUBLIC_SOCKET_URL` configuration
   - Check firewall settings

#### System Issues
1. **File Upload Not Working**
   - Check file size limits
   - Verify upload directory permissions
   - Check backend file processing

2. **Audio Processing Failed**
   - Verify Google Cloud Speech API is enabled
   - Check audio file format support
   - Verify service account permissions

### Debug Mode
```bash
# Backend debug
NODE_ENV=development yarn start:debug

# Frontend debug
NEXT_PUBLIC_DEBUG=true yarn dev
```

## 📈 Performance Optimization

### Backend Optimization
- Database query optimization
- Caching strategies
- File processing optimization
- WebSocket connection pooling

### Frontend Optimization
- Code splitting and lazy loading
- Image optimization
- Bundle size optimization
- Real-time data caching

## 🤝 Contributing

### Development Setup
1. Fork both repositories
2. Create feature branches
3. Make changes to both frontend and backend
4. Test integration
5. Submit pull requests

### Code Standards
- TypeScript for type safety
- ESLint for code quality
- Prettier for code formatting
- Jest for testing

## 📝 License

Both projects are licensed under the MIT License - see LICENSE files for details.

## 🆘 Support

For support and questions:
- Create issues in respective repositories
- Check troubleshooting section
- Contact development team
- Review documentation

---

**Quick Start Summary:**
1. Set up MySQL database
2. Configure Google Cloud APIs
3. Start backend server (`yarn start:dev`)
4. Start frontend server (`yarn dev`)
5. Access system at `http://localhost:4001`

**System Status Check:**
- Backend: `http://localhost:8787/health`
- Frontend: `http://localhost:4001`
- Database: Check MySQL connection
- APIs: Verify Google Cloud credentials
