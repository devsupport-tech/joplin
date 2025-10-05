# Deploying Joplin Server to Coolify

This guide explains how to deploy Joplin Server using Coolify.

## Quick Start

### 1. Prerequisites
- Coolify instance running
- Domain name configured (optional but recommended)

### 2. Setup Files

Copy the environment file and configure it:
```bash
cp .env.coolify .env
```

Edit `.env` and update:
- `APP_BASE_URL` - Your public URL (e.g., https://joplin.yourdomain.com)
- `POSTGRES_PASSWORD` - A strong password for the database
- Other settings as needed

### 3. Deploy to Coolify

#### Option A: Using Coolify UI

1. In Coolify, create a new **Docker Compose** resource
2. Point it to your repository (or upload the files)
3. The compose file is already named `docker-compose.yaml` (Coolify default)
4. Add environment variables from your `.env` file in Coolify's environment settings
5. Deploy!

#### Option B: Using Git Repository

1. Push this repository (including `docker-compose.yaml`) to your Git provider
2. In Coolify:
   - Create new resource → Docker Compose
   - Connect your Git repository
   - Coolify will automatically find `docker-compose.yaml`
   - Configure environment variables
   - Deploy

### 4. Configure Domain (Recommended)

In Coolify:
1. Go to your Joplin service
2. Add your domain (e.g., joplin.yourdomain.com)
3. Enable automatic HTTPS/SSL

## Environment Variables

Required variables in Coolify:

| Variable | Description | Example |
|----------|-------------|---------|
| `APP_BASE_URL` | Public URL for your server | `https://joplin.example.com` |
| `POSTGRES_PASSWORD` | Database password | `secure-random-password` |
| `POSTGRES_USER` | Database username | `joplin` (default) |
| `POSTGRES_DATABASE` | Database name | `joplin` (default) |

## Post-Deployment

### First Access
After deployment, access your Joplin server at your configured `APP_BASE_URL`.

The default admin account will be created on first run:
- Email: `admin@localhost`
- Password: `admin`

**⚠️ IMPORTANT: Change the admin password immediately after first login!**

### Client Configuration

Configure your Joplin clients to sync with your server:
1. Open Joplin (Desktop/Mobile)
2. Go to Settings → Synchronization
3. Select "Joplin Server" as sync target
4. Enter your server URL (APP_BASE_URL)
5. Enter your credentials

## Ports

- **22300** - Joplin Server (mapped to host, can be changed with APP_PORT)
- **5432** - PostgreSQL (internal only, not exposed)

## Data Persistence

Data is persisted in Docker volumes:
- `postgres-data` - PostgreSQL database
- `joplin-data` - Application data (if needed)

## Updating

To update Joplin Server:

```bash
docker-compose pull
docker-compose up -d
```

Or in Coolify UI, click the "Redeploy" button.

## Troubleshooting

### Check logs
```bash
docker-compose logs -f app
```

### Database connection issues
Verify environment variables are set correctly in Coolify.

### Port conflicts
Change `APP_PORT` in your environment variables.

## Advanced Configuration

For additional configuration options (email, storage, user limits, etc.), see:
- [Joplin Server Configuration](https://github.com/laurent22/joplin/blob/dev/readme/server/config.md)
- `.env.coolify` file for examples

## Security Recommendations

1. ✅ Use a strong `POSTGRES_PASSWORD`
2. ✅ Configure HTTPS/SSL via Coolify
3. ✅ Change default admin password immediately
4. ✅ Keep the server updated regularly
5. ✅ Consider enabling email notifications for important events

## Support

- [Joplin Forum](https://discourse.joplinapp.org/)
- [GitHub Issues](https://github.com/laurent22/joplin/issues)
- [Documentation](https://joplinapp.org/help/)
