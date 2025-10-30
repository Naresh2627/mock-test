# Deployment Guide for Render

This guide will help you deploy your Notes Authentication App to Render.

## Prerequisites

1. **GitHub Repository** - Push your code to GitHub
2. **Supabase Account** - Your database should be set up
3. **Google OAuth App** - For Google authentication
4. **Gmail App Password** - For email functionality (optional)

## Step 1: Deploy Backend (Node.js API)

### 1.1 Create New Web Service on Render

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click "New" → "Web Service"
3. Connect your GitHub repository
4. Select the repository containing your code

### 1.2 Configure Backend Service

**Basic Settings:**
- **Name**: `notes-auth-backend` (or your preferred name)
- **Environment**: `Node`
- **Region**: Choose closest to your users
- **Branch**: `main` (or your default branch)
- **Root Directory**: `auth`
- **Build Command**: `npm install`
- **Start Command**: `npm start`

### 1.3 Environment Variables for Backend

Add these environment variables in Render:

```
NODE_ENV=production
PORT=10000
JWT_SECRET_KEY=your-super-secret-jwt-key-change-in-production-make-it-long-and-random
SESSION_SECRET=your-session-secret-change-in-production-make-it-long-and-random
ENCRYPTION_KEY=your-encryption-key-change-in-production-make-it-long-and-random

# Google OAuth (Get from Google Cloud Console)
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret

# Supabase (Get from Supabase Dashboard)
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_KEY=your-supabase-anon-key

# Email (Optional - for password reset)
EMAIL_USER=your-email@gmail.com
EMAIL_PASS=your-gmail-app-password

# Frontend URL (Will be updated after frontend deployment)
FRONTEND_URL=https://your-frontend-app.onrender.com
```

**Important Security Notes:**
- Generate strong, random secrets for JWT_SECRET_KEY, SESSION_SECRET, and ENCRYPTION_KEY
- Use at least 32 characters for each secret
- Never use the example values in production

## Step 2: Deploy Frontend (React App)

### 2.1 Create New Static Site on Render

1. In Render Dashboard, click "New" → "Static Site"
2. Connect the same GitHub repository
3. Select your repository

### 2.2 Configure Frontend Service

**Basic Settings:**
- **Name**: `notes-auth-frontend` (or your preferred name)
- **Branch**: `main`
- **Root Directory**: `frontend`
- **Build Command**: `npm install && npm run build`
- **Publish Directory**: `build`

### 2.3 Environment Variables for Frontend

Add this environment variable:

```
REACT_APP_API_URL=https://your-backend-app.onrender.com
```

**Replace `your-backend-app` with your actual backend service name from Step 1.**

## Step 3: Update Configuration

### 3.1 Update Backend FRONTEND_URL

After frontend is deployed:
1. Go to your backend service in Render
2. Update the `FRONTEND_URL` environment variable with your frontend URL
3. Redeploy the backend service

### 3.2 Update Google OAuth Redirect URIs

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Navigate to your OAuth 2.0 Client
3. Add these Authorized Redirect URIs:
   ```
   https://your-backend-app.onrender.com/oauth/google/callback
   ```

### 3.3 Update CORS Origins

The backend is already configured to use the FRONTEND_URL environment variable for CORS.

## Step 4: Database Setup

### 4.1 Run Database Migrations

In your Supabase SQL Editor, run these migrations:

1. **Users table migration** (if not already done):
```sql
-- Copy contents from auth/database-migration.sql
```

2. **Notes table migration**:
```sql
-- Copy contents from auth/notes-migration.sql
```

3. **Labels table migration**:
```sql
-- Copy contents from auth/labels-migration.sql
```

## Step 5: Testing Deployment

### 5.1 Test Backend API

Visit: `https://your-backend-app.onrender.com/`

You should see:
```json
{
  "message": "Authentication API is running",
  "endpoints": { ... }
}
```

### 5.2 Test Frontend

Visit: `https://your-frontend-app.onrender.com/`

You should see the login page.

### 5.3 Test Full Flow

1. **Sign up** with email/password
2. **Login** with credentials
3. **Create a note**
4. **Test Google OAuth**
5. **Test password reset** (if email configured)

## Troubleshooting

### Common Issues:

1. **CORS Errors**: Make sure FRONTEND_URL is set correctly in backend
2. **Google OAuth Fails**: Check redirect URIs in Google Console
3. **Database Errors**: Ensure all migrations are run in Supabase
4. **Environment Variables**: Double-check all required variables are set

### Logs:

- **Backend Logs**: Available in Render dashboard under your backend service
- **Frontend Build Logs**: Available in Render dashboard under your frontend service

## Security Checklist

- [ ] Strong, unique secrets for JWT_SECRET_KEY, SESSION_SECRET, ENCRYPTION_KEY
- [ ] Google OAuth redirect URIs updated
- [ ] CORS configured correctly
- [ ] Environment variables set (no hardcoded secrets)
- [ ] Database migrations completed
- [ ] HTTPS enabled (automatic on Render)

## URLs After Deployment

- **Backend API**: `https://your-backend-app.onrender.com`
- **Frontend App**: `https://your-frontend-app.onrender.com`
- **Database**: Your Supabase project URL

Your Notes Authentication App is now live! 🎉