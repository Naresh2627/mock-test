# Render Environment Variables

## Backend Service Environment Variables

Copy these EXACTLY into your Render backend service:

```
NODE_ENV=production
PORT=10000
JWT_SECRET_KEY=leoleoloe-make-this-longer-for-production
SESSION_SECRET=royroyrooy-make-this-longer-for-production
GOOGLE_CLIENT_ID=1079850602785-p0dk3bjsjh659l5f38veakal2jbjf9bb.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=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
SUPABASE_URL=https://saeyfrvjvgtorgnvkfvf.supabase.co
SUPABASE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InNhZXlmcnZqdmd0b3JnbnZrZnZmIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTkyMTIxNzksImV4cCI6MjA3NDc4ODE3OX0.lbJ6Vmo9gPGvg85kvGiYh9oW7gO-zqH0K-z0IRY22-8
EMAIL_USER=dummy@test.com
EMAIL_PASS=dummy-password
ENCRYPTION_KEY=your-super-secret-encryption-key-change-in-production-make-it-32-chars-minimum
FRONTEND_URL=https://your-frontend-app-name.onrender.com
BACKEND_URL=https://your-backend-app-name.onrender.com
```

## Frontend Service Environment Variables

Copy this into your Render frontend static site:

```
REACT_APP_API_URL=https://your-backend-app-name.onrender.com
```

## Important Notes:

1. Replace `your-frontend-app-name` and `your-backend-app-name` with your actual Render service names
2. Update Google OAuth redirect URI in Google Console to: `https://your-backend-app-name.onrender.com/oauth/google/callback`
3. Make sure all database migrations are run in Supabase
4. The backend service should be deployed first to get the URL for frontend

## Common Deployment Errors and Fixes:

### Error: "Cannot find module"
- Make sure all dependencies are in package.json
- Check that import statements use correct paths

### Error: "CORS policy"
- Verify FRONTEND_URL is set correctly in backend
- Check that frontend is using correct REACT_APP_API_URL

### Error: "Google OAuth fails"
- Update redirect URI in Google Console
- Verify GOOGLE_CLIENT_ID and GOOGLE_CLIENT_SECRET are correct

### Error: "Database connection failed"
- Check SUPABASE_URL and SUPABASE_KEY are correct
- Ensure database migrations are run

### Error: "Session/Cookie issues"
- Backend now configured for production cookies
- Make sure both services use HTTPS (automatic on Render)