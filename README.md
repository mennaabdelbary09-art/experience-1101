# experience-1101
# Returno Face Recognition System

## 🎯 Competition-Ready Backend Implementation

A comprehensive missing person tracking system with AI-powered face and voice recognition, featuring human-in-the-loop controls, PII encryption, and ethical AI practices.

---

## 📋 Table of Contents

- [Features](#features)
- [System Architecture](#system-architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Database Setup](#database-setup)
- [Running the Application](#running-the-application)
- [API Documentation](#api-documentation)
- [Testing](#testing)
- [Security Features](#security-features)
- [Ethical AI & Privacy](#ethical-ai--privacy)
- [Competition Compliance](#competition-compliance)
- [Production Deployment](#production-deployment)
- [Troubleshooting](#troubleshooting)

---

## ✨ Features

### Core Functionality
- ✅ **Missing Person Reports**: Submit detailed reports with photos and voice recordings
- ✅ **AI-Powered Matching**: Automatic face and voice recognition with 70% confidence threshold
- ✅ **Human-in-the-Loop**: Admin approval required before sending notifications
- ✅ **Multi-Modal Recognition**: Face + Voice recognition for higher accuracy
- ✅ **Real-time Search**: Search by photo, name, or live camera feed

### Security & Privacy
- 🔒 **PII Encryption**: All sensitive data encrypted at rest (national IDs, phone numbers)
- 🔒 **Audit Logging**: Complete audit trail of all critical actions
- 🔒 **SQL Injection Protection**: Parameterized queries and input validation
- 🔒 **XSS Prevention**: Input sanitization and output encoding
- 🔒 **Data Retention**: Automatic deletion of closed cases after 10 days

### Ethical AI Features
- 🤖 **Confidence Scoring**: AI predictions include confidence metrics and risk indicators
- 🤖 **False Positive Detection**: Automatic flagging of high-risk matches
- 🤖 **Duplicate Prevention**: Prevents redundant notifications
- 🤖 **Consent Tracking**: Comprehensive consent logging for data usage
- 🤖 **Human Oversight**: AI is assistive only - never autonomous

### User Roles
- 👤 **Public Reporter**: Submit missing person reports
- 👨‍👩‍👧 **Guardian**: Manage reports for minors
- 🛡️ **Admin**: Review matches, approve notifications, manage system

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    React Frontend                        │
│                   (Port 3000)                            │
└────────────────────┬────────────────────────────────────┘
                     │ REST API (JSON)
                     │
┌────────────────────▼────────────────────────────────────┐
│                  Flask Backend                           │
│                   (Port 5000)                            │
│  ┌─────────────────────────────────────────────────┐   │
│  │  Routes Layer (API Endpoints)                    │   │
│  └─────────────────┬────────────────────────────────┘   │
│  ┌─────────────────▼────────────────────────────────┐   │
│  │  Services Layer (Business Logic)                 │   │
│  │  - AI Service (Face/Voice Recognition)           │   │
│  │  - Matching Service                              │   │
│  │  - Notification Service                          │   │
│  └─────────────────┬────────────────────────────────┘   │
│  ┌─────────────────▼────────────────────────────────┐   │
│  │  Models Layer (Database ORM)                     │   │
│  │  - User, MissingReport, MatchResult              │   │
│  │  - Notification, ConsentLog, AuditLog            │   │
│  └─────────────────┬────────────────────────────────┘   │
└────────────────────┼────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│              SQLite Database                             │
│           (database/returno.db)                          │
└──────────────────────────────────────────────────────────┘

External Components:
┌─────────────────────┐  ┌─────────────────────┐
│  TensorFlow Models  │  │   File Storage      │
│  - Face Recognition │  │   - Photos          │
│  - Voice Recognition│  │   - Voice Clips     │
└─────────────────────┘  └─────────────────────┘
```

---

## 📦 Prerequisites

- **Python**: 3.8 or higher
- **pip**: Latest version
- **Virtual Environment**: Recommended
- **Operating System**: Windows, macOS, or Linux
- **RAM**: Minimum 4GB (8GB recommended for AI models)
- **Disk Space**: 2GB free space

---

## 🚀 Installation

### Step 1: Clone or Download the Project

```bash
# If using git
git clone <your-repo-url>
cd returno-system

# Or extract the downloaded ZIP file
```

### Step 2: Create Virtual Environment

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

**Note**: TensorFlow installation may take several minutes.

### Step 4: Create Directory Structure

```bash
python setup_structure.py
```

---

## ⚙️ Configuration

### Step 1: Create Environment File

Copy the template and configure:

```bash
cp .env.template .env
```

### Step 2: Generate Encryption Key

```python
# Run in Python console
from cryptography.fernet import Fernet
print(Fernet.generate_key().decode())
```

Copy the output and paste into `.env` as `ENCRYPTION_KEY`

### Step 3: Configure Essential Settings

Edit `.env`:

```bash
# Change these for security
SECRET_KEY=your-random-secret-key-min-32-chars
JWT_SECRET_KEY=your-jwt-secret-key-min-32-chars
ENCRYPTION_KEY=<generated-key-from-step-2>

# Database path
DATABASE_URL=sqlite:///database/returno.db

# AI Model paths
FACE_MODEL_PATH=models/face_recognition/saved_model.h5
VOICE_MODEL_PATH=models/voice_recognition/voice_model.h5

# Confidence thresholds
AI_CONFIDENCE_THRESHOLD=0.70
MATCH_CONFIDENCE_THRESHOLD=0.75
```

---

## 🗄️ Database Setup

### Initialize Database

```bash
python scripts/init_database.py
```

This will:
- Create all database tables
- Set up indexes and relationships
- Create default admin user (username: `admin`, password: `admin123`)

**⚠️ IMPORTANT**: Change the default admin password immediately!

### Verify Database

```bash
python scripts/check_database.py
```

### Database Schema

The system includes these main tables:
- `users` - User accounts and authentication
- `missing_reports` - Missing person reports
- `file_attachments` - Photos and voice recordings
- `match_results` - AI matching results
- `notifications` - Notification queue
- `admin_approvals` - Human-in-the-loop approvals
- `consent_logs` - Data usage consent tracking
- `ai_confidence_metrics` - AI performance metrics
- `audit_logs` - Comprehensive audit trail
- `success_stories` - Public success stories

---

## 🎮 Running the Application

### Development Mode

```bash
# Activate virtual environment first
# Windows: venv\Scripts\activate
# macOS/Linux: source venv/bin/activate

python app.py
```

The server will start on `http://localhost:5000`

### Frontend Integration

Ensure your React frontend is running on `http://localhost:3000` and pointing to the backend:

```javascript
const API_URL = 'http://localhost:5000/api';
```

### Verify Backend

Open browser: `http://localhost:5000/api/health`

Expected response:
```json
{
  "status": "healthy",
  "version": "1.0.0",
  "database": "connected"
}
```

---

## 📡 API Documentation

### Authentication

#### Register User
```http
POST /api/auth/register
Content-Type: application/json

{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "SecurePass123",
  "full_name": "John Doe",
  "phone": "+1234567890"
}
```

#### Login
```http
POST /api/auth/login
Content-Type: application/json

{
  "username": "john_doe",
  "password": "SecurePass123"
}
```

Response:
```json
{
  "success": true,
  "token": "eyJ0eXAiOiJKV1QiLCJhbGc...",
  "user": {
    "id": 1,
    "username": "john_doe",
    "role": "public_reporter"
  }
}
```

### Missing Person Reports

#### Submit Report
```http
POST /api/report_missing
Content-Type: application/json
Authorization: Bearer <token>

{
  "guardian_name": "Jane Doe",
  "guardian_age": 35,
  "guardian_national_id": "12345678901234",
  "guardian_phone": "+1234567890",
  "relationship": "Mother",
  "missing_name": "Child Doe",
  "missing_age": 8,
  "last_seen_location": "Central Park, NYC",
  "last_seen_date": "2026-01-05T14:30:00",
  "description": "Wearing blue jacket...",
  "photos": ["data:image/jpeg;base64,...", "..."]
}
```

#### Get All Reports (Admin Only)
```http
GET /api/admin/reports
Authorization: Bearer <admin-token>
```

#### Search by Name
```http
GET /api/search_by_name?name=John
```

### AI Recognition

#### Search by Photo
```http
POST /api/search_by_photo
Content-Type: application/json

{
  "photo": "data:image/jpeg;base64,..."
}
```

Response:
```json
{
  "success": true,
  "predicted_identity": "John Doe",
  "confidence": 0.87,
  "matches": [
    {
      "report_id": 123,
      "confidence": 0.87,
      "match_type": "face"
    }
  ]
}
```

### Admin Operations

#### Review Match
```http
POST /api/admin/review_match
Content-Type: application/json
Authorization: Bearer <admin-token>

{
  "match_id": 45,
  "action": "approve",
  "notes": "High confidence match verified"
}
```

#### Approve Notification
```http
POST /api/admin/approve_notification
Content-Type: application/json
Authorization: Bearer <admin-token>

{
  "notification_id": 12,
  "action": "approve"
}
```

---

## 🧪 Testing

### Run Unit Tests

```bash
pytest tests/unit/ -v
```

### Run Integration Tests

```bash
pytest tests/integration/ -v
```

### Run All Tests with Coverage

```bash
pytest --cov=app --cov-report=html
```

View coverage report: `htmlcov/index.html`

### Test Face Recognition

```bash
python tests/test_face_recognition.py
```

---

## 🔐 Security Features

### 1. PII Encryption
All sensitive fields are encrypted using Fernet symmetric encryption:
- National IDs
- Phone numbers
- Email addresses
- Guardian information

### 2. SQL Injection Prevention
- All queries use parameterized statements
- Input validation on all endpoints
- Pattern matching for dangerous SQL keywords

### 3. XSS Prevention
- HTML tag stripping
- Special character escaping
- Content Security Policy headers

### 4. Authentication & Authorization
- JWT token-based authentication
- Role-based access control (RBAC)
- Password hashing with bcrypt

### 5. Audit Logging
All critical actions are logged:
- User login/logout
- Report creation/modification
- Match approvals
- Notification sending
- Admin actions

Log files:
- `logs/returno.log` - General application logs
- `logs/audit.log` - Audit trail
- `logs/security.log` - Security events

---

## 🤖 Ethical AI & Privacy

### Human-in-the-Loop Controls

**AI Limitations**:
- AI predictions are **assistive only**, never autonomous
- All match notifications require explicit admin approval
- System includes confidence thresholds and risk indicators
- False positive detection and flagging

**Admin Dashboard**:
- Review AI match results
- Approve/reject notifications
- View confidence scores and risk factors
- Access complete audit trail

### Data Privacy

**Consent Management**:
- Explicit consent required for data processing
- Consent logs with timestamps
- Right to revoke consent
- Transparent data usage

**Data Minimization**:
- Only collect necessary information
- Auto-delete closed cases after 10 days
- Soft-delete with audit trail
- Export/delete user data on request

**Public Privacy**:
- PII hidden from public view
- Success stories anonymized
- No public search of active reports

### AI Transparency

**Confidence Metrics**:
- Primary confidence score
- Secondary confidence (alternative matches)
- Confidence delta (certainty indicator)
- Processing time and model version

**Risk Indicators**:
- Low confidence warning
- Small confidence delta alert
- Unknown identity flag
- Image/audio quality scores

---

## 🏆 Competition Compliance

### Zero-Cost Requirements
✅ Uses only free, pip-installable packages
✅ SQLite database (no cloud costs)
✅ Local file storage (no S3/cloud)
✅ No external APIs or services
✅ Runs entirely on localhost

### Scalability Notes

**Current: SQLite (Competition)**
- File-based database
- Perfect for demo and development
- Single-server deployment
- Concurrent access: ~10 users

**Future: PostgreSQL (Production)**
- Scale to thousands of users
- Better concurrent access
- Built-in replication
- Cloud-ready (AWS RDS, Azure SQL)

**Migration Path**:
1. Change `DATABASE_URL` in `.env`
2. Run migrations: `flask db upgrade`
3. No code changes needed!

### Production Enhancements

When deploying to production:

1. **Database**:
   - Switch to PostgreSQL
   - Enable connection pooling
   - Set up read replicas

2. **Security**:
   - Use environment-based secrets management
   - Enable HTTPS/SSL
   - Implement rate limiting
   - Add WAF (Web Application Firewall)

3. **Storage**:
   - Use S3 or Azure Blob for files
   - Implement CDN for images
   - Add backup strategy

4. **Monitoring**:
   - APM tools (New Relic, DataDog)
   - Error tracking (Sentry)
   - Performance monitoring

5. **Infrastructure**:
   - Docker containerization
   - Kubernetes orchestration
   - Load balancing
   - Auto-scaling

---

## 🐛 Troubleshooting

### Database Locked Error

**Problem**: `sqlite3.OperationalError: database is locked`

**Solution**:
```bash
# Close all connections
# Delete database lock
rm database/returno.db-wal
rm database/returno.db-shm

# Restart server
```

### Model Not Found

**Problem**: `Face recognition model not found`

**Solution**:
```bash
# Copy your model file
cp saved_model.h5 models/face_recognition/
```

### Encryption Errors

**Problem**: `Invalid token - decryption failed`

**Solution**:
```bash
# Generate new key
python -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())"

# Update .env
# Re-encrypt existing data or recreate database
```

### Port Already in Use

**Problem**: `Address already in use`

**Solution**:
```bash
# Windows
netstat -ano | findstr :5000
taskkill /PID <process-id> /F

# macOS/Linux
lsof -ti:5000 | xargs kill -9
```

### Import Errors

**Problem**: `ModuleNotFoundError`

**Solution**:
```bash
# Reinstall dependencies
pip install -r requirements.txt --force-reinstall
```

---

## 📝 License

This project is developed for educational and competition purposes.

---

## 👥 Contributors

- Development Team
- AI Research Team
- Security Review Team

---

## 📞 Support

For issues or questions:
1. Check troubleshooting section
2. Review logs in `logs/` directory
3. Check database health: `python scripts/check_database.py`

---

## 🎓 Learning Resources

- **Flask**: https://flask.palletsprojects.com/
- **SQLAlchemy**: https://docs.sqlalchemy.org/
- **TensorFlow**: https://www.tensorflow.org/
- **Security Best Practices**: https://owasp.org/

---

**Built with ❤️ for making communities safer**
