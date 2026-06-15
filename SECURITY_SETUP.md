# Security Setup Guide

This document outlines the sensitive files and how to configure them properly.

## Environment Variables

### Client (.env)
Copy `.env.example` to `.env` and fill in your actual values:
```bash
cp .env.example .env
```

Update with your values:
- `VITE_BASE_URL` - Your API Gateway endpoint
- `VITE_GOOGLE_CLIENT_ID` - Your Google OAuth client ID
- `VITE_AWS_USER_POOL_ID` - AWS Cognito User Pool ID
- `VITE_AWS_USER_POOL_WEB_CLIENT_ID` - AWS Cognito Web Client ID

### Services (.env)
Copy `.env.example` to `.env` and fill in your actual values:
```bash
cd services
cp .env.example .env
```

Update with your values:
- AWS credentials and configuration
- Database connection details
- API keys and secrets
- Google Cloud credentials path

## Google Service Account Credentials

The `services/src/user/credentials.json` file contains sensitive Google Cloud service account credentials.

### Setup:
1. Copy `credentials.json.example` to `credentials.json`
2. Download your actual service account key from Google Cloud Console
3. Replace the template values with your actual key

**IMPORTANT**: Never commit the actual `credentials.json` file to version control!

## AWS Credentials

Store AWS credentials using one of these methods:
1. AWS credentials file (`~/.aws/credentials`)
2. Environment variables (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`)
3. IAM roles (recommended for production)

## Sensitive Files in .gitignore

The following files are excluded from version control:
- `.env` files (all variants)
- `credentials.json`
- `*.pem`, `*.key` (certificate/key files)
- `secrets.json`
- `aws-credentials.json`

## Before Pushing to Remote

Ensure:
1. ✅ No `.env` files are committed (only `.env.example`)
2. ✅ No credential JSON files are committed (only `.example` versions)
3. ✅ No private keys or certificates are committed
4. ✅ All sensitive data in test files is replaced with placeholders

Run this to verify:
```bash
git status
```

## To Remove Already Committed Sensitive Files

If sensitive files were already committed:
```bash
# Remove from git history (use with caution)
git filter-branch --tree-filter 'rm -f services/src/user/credentials.json' HEAD
git push --force-with-lease

# Or use BFG Repo-Cleaner (recommended)
bfg --delete-files credentials.json
```

See: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository
