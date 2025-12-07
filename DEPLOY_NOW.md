# Quick Netlify Deployment Steps

## Your frontend is ready to deploy! ✅

The `dist` folder has been built and is ready.

## Deploy Now:

### Option 1: Drag & Drop (Easiest)
1. Go to https://app.netlify.com/drop
2. Drag the `client/dist` folder onto the page
3. Your site will be live in seconds!
4. **IMPORTANT**: Go to Site settings > Environment variables
5. Add: `VITE_API_URL` = `https://your-backend-url.com`
6. Redeploy for changes to take effect

### Option 2: GitHub + Auto Deploy
1. Push your code to GitHub
2. Go to https://app.netlify.com/
3. Click "Add new site" > "Import an existing project"
4. Select GitHub and choose your repository
5. Build settings:
   - Base directory: `client`
   - Build command: `npm run build`
   - Publish directory: `client/dist`
6. Environment variables:
   - `VITE_API_URL` = `https://your-backend-url.com`
7. Click "Deploy"

### Option 3: Netlify CLI
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Login to Netlify
netlify login

# Deploy from client directory
cd client
netlify deploy --prod --dir=dist
```

## Before Deploying - Deploy Your Backend First!

Your backend (server folder) needs to be deployed separately. Recommended options:

### Render.com (Best for this project):
1. Go to https://render.com/
2. Click "New +" > "Web Service"
3. Connect your repository
4. Settings:
   - Root Directory: `server`
   - Build Command: `npm install`
   - Start Command: `npm start`
5. Add environment variables:
   - `DATABASE_URL` (if using PostgreSQL)
   - `JWT_SECRET` (your secret key)
   - `PORT` = `3001`
6. Click "Create Web Service"
7. Copy the URL (e.g., `https://your-app.onrender.com`)

### Railway.app:
1. Go to https://railway.app/
2. Click "New Project" > "Deploy from GitHub"
3. Select repository and choose `server` folder
4. Add PostgreSQL database (or use SQLite)
5. Set environment variables
6. Deploy and copy the URL

### Update Backend URL:
Once backend is deployed, update `client/.env.production`:
```
VITE_API_URL=https://your-backend-url.com
```

Then rebuild and redeploy frontend:
```bash
cd client
npm run build
netlify deploy --prod --dir=dist
```

## Important Backend Setup:

Update CORS in `server/src/index.js`:
```javascript
app.use(cors({
  origin: [
    'http://localhost:5173',
    'https://your-netlify-site.netlify.app'
  ],
  credentials: true
}));
```

## Checklist:
- [ ] Backend deployed (Render/Railway/Heroku)
- [ ] Backend URL copied
- [ ] Update `.env.production` with backend URL
- [ ] Rebuild frontend: `npm run build`
- [ ] Update CORS in backend with Netlify URL
- [ ] Deploy frontend to Netlify
- [ ] Test login and API calls

Your site will be at: `https://[your-site-name].netlify.app`
