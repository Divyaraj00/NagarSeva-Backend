# Complaint Management System - Backend API

## 🎯 Overview

A robust backend API for managing civic complaints with automatic SLA (Service Level Agreement) tracking, employee assignment, and real-time notifications.

## ✨ Features

- **Complaint Management**: Submit, track, and resolve civic complaints
- **SLA Tracking**: Automatic deadline calculation and violation detection
- **Smart Assignment**: Auto-assign complaints to employees based on workload
- **Employee Dashboard**: Track assigned complaints and SLA status
- **Manager Dashboard**: Department-wide analytics and team management
- **Real-time Notifications**: SLA violation and warning alerts
- **Heatmap Data**: Geographic visualization of complaints
- **Analytics**: Comprehensive statistics and reporting

## 🛠️ Tech Stack

- **Runtime**: Node.js (v18+)
- **Framework**: Express.js
- **Database**: Supabase (PostgreSQL)
- **Storage**: Supabase Storage (for complaint images)
- **Authentication**: Supabase Auth

## 📋 Prerequisites

- Node.js v18 or higher
- Supabase account and project
- Git installed

## 🚀 Quick Start

### 1. Clone Repository

```bash
git clone https://github.com/YOUR_USERNAME/complaint-management-backend.git
cd complaint-management-backend
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment Variables

Create a `.env` file from the template:

```bash
cp .env.example .env
```

Edit `.env` with your Supabase credentials:

```env
PORT=3000
NODE_ENV=development

SUPABASE_URL=your_supabase_project_url
SUPABASE_KEY=your_supabase_anon_key
```

### 4. Run Development Server

```bash
npm run dev
```

API will be available at `http://localhost:3000`

## 📚 API Documentation

### Base URL
- **Development**: `http://localhost:3000`
- **Production**: `https://your-app.onrender.com`

### Endpoints

#### Complaint Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/complaint` | Submit new complaint |
| GET | `/complaints/user/:userId` | Get user's complaints |
| GET | `/complaint/:id` | Get complaint details |
| PATCH | `/complaint/:Cid/toggle` | Toggle complaint status |

#### Manager Dashboard

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/manager/:Eid/profile` | Get manager profile |
| GET | `/manager/:Eid/workers` | Get department workers |
| GET | `/manager/:Eid/complaints` | Get department complaints |
| GET | `/manager/:Eid/stats` | Get department statistics |
| GET | `/manager/:Eid/sla-violations` | Get SLA violations |
| POST | `/manager/assign-complaint` | Assign complaint to worker |
| DELETE | `/manager/complaint/:Cid` | Delete complaint |

#### Employee Dashboard

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/employee/:Eid/complaints` | Get assigned complaints |
| GET | `/employee/:Eid/sla-violations` | Get employee SLA violations |

#### Analytics & Mapping

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/analytics` | Get system-wide analytics |
| GET | `/leaflet` | Get pending complaints coordinates |
| GET | `/manager/:Eid/heatmap` | Get department heatmap data |

#### Notifications

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/notifications/:Eid` | Get user notifications |
| PATCH | `/notifications/:id/read` | Mark notification as read |

#### SLA Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/trigger-sla-check` | Manually trigger SLA check |

## 🗃️ Database Schema

### Tables Created

#### `complaints`
- Added: `Deadline`, `SLAStatus`, `SLAViolatedAt`, `TimeToResolve`

#### `Department`
- Added: `SLAHours` (deadline hours for each department)

#### `sla_notifications`
- New table for SLA notifications

See deployment guide for SQL setup scripts.

## 🔒 Security

- Environment variables for sensitive data
- CORS enabled for frontend integration
- Supabase RLS (Row Level Security)
- Request body size limits (50mb)
- Input validation

## 🌐 Deployment

### Deploy to Render

1. Push code to GitHub
2. Connect repository to Render
3. Set environment variables in Render dashboard
4. Deploy!

See [DEPLOYMENT_GUIDE.md](./DEPLOYMENT_GUIDE.md) for detailed instructions.

## 🧪 Testing

Test the API health:

```bash
curl http://localhost:3000/
```

Expected response:
```json
{
  "status": "ok",
  "message": "Welcome to API with SLA Tracking",
  "timestamp": "2025-01-10T12:00:00.000Z"
}
```

## 📊 SLA Configuration

Default SLA hours by department:
- **Electrical**: 24 hours
- **Water**: 48 hours
- **Public Infrastructure**: 72 hours
- **Cleanliness**: 36 hours

Update in Supabase `Department` table if needed.

## 🔄 Auto-Assignment Logic

1. New complaint submitted → Assign to employee with < 5 tasks
2. Employee completes task → Auto-assign pending complaint if available
3. Manager can manually reassign complaints
4. Maximum 5 active complaints per employee

## 🚨 SLA Tracking

- **On Track**: More than 4 hours until deadline
- **Warning**: Less than 4 hours until deadline
- **Violated**: Deadline has passed
- **Completed**: Complaint resolved

### Automatic Checking

Edge function runs every 30 minutes to check SLA status and create notifications.

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

## 📝 Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `PORT` | Server port | No (default: 3000) |
| `NODE_ENV` | Environment | No (default: development) |
| `SUPABASE_URL` | Supabase project URL | Yes |
| `SUPABASE_KEY` | Supabase anon key | Yes |
| `SUPABASE_SERVICE_ROLE_KEY` | Service role key (for edge functions) | Optional |

## 🐛 Troubleshooting

### Common Issues

**Issue**: Module not found errors
```bash
npm install
```

**Issue**: Database connection failed
- Verify Supabase credentials in `.env`
- Check Supabase project is active

**Issue**: Port already in use
```bash
# Change PORT in .env or kill process
lsof -ti:3000 | xargs kill
```

## 📞 Support

For issues and questions:
- Open an issue on GitHub
- Check existing issues for solutions


