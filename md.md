
---
title: "Manual Deployment of a Node.js Application to a Linux Server"
sidebarTitle: "Manual Deployment"
description: "Deploy a Node.js Express app to a Linux VPS using SSH, Git, npm, and PM2 with step-by-step instructions, zero-downtime reloads, and rollback procedures."
---

Manual deployment is the process of getting your application onto a production server without a CI/CD pipeline. Understanding every step makes you a better engineer and helps you debug deployment failures. This page walks through a complete deployment from SSH login to a running application, plus updating and rolling back.

## Manual vs Automated Deployment

| Approach | How | Best For |
| --- | --- | --- |
| **Manual** | SSH in, pull code, restart | Learning, small projects, quick fixes |
| **CI/CD** | GitHub Actions, push triggers deploy | Teams, multiple environments, audit trails |

Even if you use CI/CD in production, understanding manual deployment helps you debug pipeline failures.

## Prerequisites

Before deploying, ensure the server has:

- Node.js installed (via NVM)
- PM2 installed globally
- Nginx installed and configured
- Your .env variables ready to set on the server
- SSH access to the server

## Complete First Deployment

<Steps>
  <Step title="SSH into the server">
    ```bash
    ssh ubuntu@your-server-ip
    # Or with a key file:
    ssh -i ~/.ssh/mykey.pem ubuntu@your-server-ip
    ```
  </Step>
  <Step title="Navigate to the apps directory">
    ```bash
    cd /var/www
    sudo mkdir myapp
    sudo chown ubuntu:ubuntu myapp
    ```
  </Step>
  <Step title="Clone the repository">
    ```bash
    git clone https://github.com/yourusername/your-repo.git myapp
    cd myapp
    ```
  </Step>
  <Step title="Install production dependencies only">
    ```bash
    npm install --production
    # Or with npm ci (faster, uses lock file exactly)
    npm ci --only=production
    ```
  </Step>
  <Step title="Create the .env file">
    ```bash
    # Copy securely from your local machine
    # On LOCAL machine (not server):
    scp .env.production ubuntu@your-server-ip:/var/www/myapp/.env
    
    # Or manually create on server:
    nano .env
    # Paste variables, save with Ctrl+X
    ```
  </Step>
  <Step title="Start with PM2">
    ```bash
    pm2 start ecosystem.config.js --env production
    # Or simply:
    pm2 start app.js --name myapp
    ```
  </Step>
  <Step title="Save PM2 process list">
    ```bash
    pm2 save   # Persists across server reboots
    ```
  </Step>
  <Step title="Verify the application is running">
    ```bash
    pm2 list
    pm2 logs myapp --lines 20
    curl http://localhost:3000/health
    ```
  </Step>
</Steps>

## Updating the Application

After pushing code changes to GitHub, update the server:

```bash
# SSH into server
ssh ubuntu@your-server-ip
cd /var/www/myapp

# Pull latest code
git pull origin main

# Install any new dependencies
npm install --production

# Zero-downtime reload (preferred over restart)
pm2 reload myapp

# Verify
pm2 logs myapp --lines 20
curl http://localhost:3000/health
```

### pm2 restart vs pm2 reload

| Command | Behavior | Use When |
| --- | --- | --- |
| `pm2 restart` | Stops app, starts fresh (brief downtime) | Config changes that need clean start |
| `pm2 reload` | Rolling restart, zero downtime | Code changes, normal updates |

## Rollback Procedure

If a deployment causes errors:

```bash
# Find the last working commit
git log --oneline -10

# Example output:
# a1b2c3d Add rate limiting
# e4f5g6h Fix user validation (last known good)
# ...

# Roll back to last known good commit
git checkout e4f5g6h

# Restart
pm2 reload myapp

# Verify
curl http://localhost:3000/health
```

If the rollback fixes the issue, create a new commit that reverts the bad change:

```bash
git revert a1b2c3d  # Creates a new "undo" commit (safer than git reset)
git push origin main
```

## Common Deployment Errors and Fixes

| Error | Cause | Fix |
| --- | --- | --- |
| `EADDRINUSE: address already in use` | Port is occupied by another process | `pm2 list` and `pm2 delete` the old process. Or `lsof -i :3000` to find and kill it. |
| `MODULE_NOT_FOUND: Cannot find module 'express'` | Forgot to run npm install | Run `npm install --production` |
| `MongoServerError: ECONNREFUSED` | Wrong DB_URI or database is not running | Check DB_URI in .env; test connectivity with `mongosh "your-uri"` |
| `Error: Missing required env vars: JWT_SECRET` | .env file missing or not found | Verify .env exists in app directory: `ls -la .env` |
| `SyntaxError: Unexpected token` | Deployed bad code | Roll back to previous commit |
| `PM2: Application restarting too many times` | App crashing on startup | Check logs: `pm2 logs myapp --err` |

## Deployment Checklist

```bash
# On server before deploying:
pm2 list                         # Note current state
git status                       # Confirm clean working tree

# Deploy:
git pull origin main
npm install --production
pm2 reload myapp

# Verify:
pm2 list                         # All apps show 'online' status
pm2 logs myapp --lines 20        # No errors in logs
curl http://localhost:3000/health # Returns 200
```

## Full Deployment Script

Save this as `deploy.sh` and run it on the server:

```bash
#!/bin/bash
set -e  # Exit immediately on any error

APP_DIR="/var/www/myapp"
APP_NAME="myapp"

echo "Starting deployment at $(date)"

cd $APP_DIR

echo "Pulling latest code..."
git pull origin main

echo "Installing dependencies..."
npm ci --only=production

echo "Reloading application..."
pm2 reload $APP_NAME

echo "Waiting for app to start..."
sleep 3

echo "Running health check..."
RESPONSE=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/health)

if [ "$RESPONSE" = "200" ]; then
  echo "Deployment successful! Health check passed."
else
  echo "Health check failed! HTTP $RESPONSE. Check pm2 logs."
  exit 1
fi

echo "Deployment complete at $(date)"
```

Run:

```bash
chmod +x deploy.sh
./deploy.sh
```

## Key Terms

| Term | Definition |
| --- | --- |
| **Deployment** | The process of making a new version of the application available on a server |
| **SSH** | Secure Shell. Encrypted protocol for remote server access. |
| **git pull** | Downloads and merges the latest commits from the remote repository |
| **npm ci** | Like npm install but faster, deterministic, and uses package-lock.json exactly |
| **pm2 reload** | Rolling restart that avoids downtime by starting new processes before killing old ones |
| **Rollback** | Reverting to a previous known-good version after a bad deployment |
| **Zero-downtime** | Deployment technique where the application remains available throughout the update |

## Common Mistakes

<Accordion title="Pushing the .env file to Git">
  Never commit .env to your repository. Copy it to the server securely using `scp` or by pasting values into a file created with `nano .env` directly on the server.
</Accordion>

<Accordion title="Using npm install --save in production">
  Running npm install without --production installs devDependencies (jest, nodemon, eslint) on the production server. Use `npm install --production` or `npm ci --only=production` to keep the server lean.
</Accordion>

<Accordion title="Not running npm install after pulling">
  If your pull added new packages to package.json, they won't be available until you run npm install. Always include npm install in your update procedure.
</Accordion>

<Accordion title="Using pm2 restart instead of pm2 reload">
  `pm2 restart` causes a brief downtime. `pm2 reload` performs a rolling restart with zero downtime. Always use `pm2 reload` for production code updates.
</Accordion>
