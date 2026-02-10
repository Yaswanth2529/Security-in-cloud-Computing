# Deployment Guide

## Local Development

```bash
# Setup
./manage.sh setup

# Run
./manage.sh run
```

## Docker Deployment

```bash
# Build
docker-compose build

# Run
docker-compose up -d

# Logs
docker-compose logs -f web
```

## Production on Ubuntu

```bash
# Install system packages
sudo apt-get update
sudo apt-get install -y python3.11 postgresql nginx supervisor

# Create app directory
sudo mkdir -p /opt/cloud-security
cd /opt/cloud-security

# Clone and setup
git clone <repo> .
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Configure PostgreSQL
sudo -u postgres createdb cloud_security
sudo -u postgres createuser -P cloud_user

# Run migrations
python manage.py migrate

# Collect static files
python manage.py collectstatic --noinput

# Start services
systemctl restart nginx
supervisorctl reread
supervisorctl update
supervisorctl start all
```

## AWS Deployment

See AWS_DEPLOYMENT.md for detailed instructions on deploying to AWS Lambda, EC2, or ECS.

## SSL/TLS Certificates

```bash
# Using Let's Encrypt
sudo certbot --nginx -d yourdomain.com

# Auto-renewal
sudo systemctl enable certbot.timer
sudo systemctl start certbot.timer
```

## Monitoring

Set up monitoring using:
- CloudWatch (AWS)
- Sentry (Error tracking)
- New Relic (Performance)
- ELK Stack (Logging)
