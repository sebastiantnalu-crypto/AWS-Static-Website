# 🚀 Step-by-Step: Deploy Static Website to AWS S3 with GitHub Actions

---

# Step 1 — Create GitHub Repository

1. Go to GitHub
2. Click **New Repository**
3. Name:

```
aws-static-website
```

4. Choose:

* Public or Private
* Add README ✅

5. Click **Create repository**

---

# Step 2 — Add Website Files

Inside the repository:

Click:

```
Add file → Create new file
```

File name:

```
index.html
```

Paste:

```html
<!DOCTYPE html>
<html>
<head>
    <title>My CI/CD Website</title>
</head>
<body>
    <h1>Hello from CI/CD 🚀</h1>
    <p>If you see this, my pipeline works!</p>
</body>
</html>
```

Click:

```
Commit changes
```

---

# Step 3 — Create AWS S3 Bucket

Login to AWS Console.

Go to:

```
Services → S3
```

Click:

```
Create bucket
```

Bucket name example:

```
my-cicd-website-12345
```

Region:

```
Europe (Stockholm) eu-north-1
```

Uncheck:

```
Block all public access
```

Check:

```
I acknowledge public access
```

Click:

```
Create bucket
```

---

# Step 4 — Enable Static Website Hosting

Open the bucket.

Go to:

```
Properties
```

Scroll down to:

```
Static website hosting
```

Click:

```
Edit
```

Select:

```
Enable
```

Index document:

```
index.html
```

Save.

---

# Step 5 — Add Bucket Policy (Make Website Public)

Go to:

```
Permissions → Bucket Policy
```

Paste:

Replace `YOUR_BUCKET_NAME`.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
```

Click:

```
Save
```

---

# Step 6 — Create AWS Access Keys

Go to AWS:

```
IAM → Users
```

Click your user.

Open:

```
Security credentials
```

Click:

```
Create access key
```

Select:

```
Command Line Interface (CLI)
```

Create key.

Save:

```
Access Key ID
Secret Access Key
```

Important ⚠️

You will only see the secret once.

---

# Step 7 — Add GitHub Secrets

Go to your repository.

Open:

```
Settings → Secrets and variables → Actions
```

Click:

```
New repository secret
```

Create:

### Secret 1

Name:

```
AWS_ACCESS_KEY_ID
```

Value:

```
your access key
```

---

### Secret 2

Name:

```
AWS_SECRET_ACCESS_KEY
```

Value:

```
your secret key
```

---

### Secret 3

Name:

```
AWS_REGION
```

Value:

```
eu-north-1
```

---

# Step 8 — Create GitHub Actions Pipeline

Inside repository click:

```
Add file → Create new file
```

File name:

```
.github/workflows/deploy.yml
```

Paste:

```yaml
name: Deploy to S3

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:

      - name: Checkout code
        uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: Deploy to S3
        run: aws s3 sync . s3://YOUR_BUCKET_NAME --delete
```

Replace:

```
YOUR_BUCKET_NAME
```

Click:

```
Commit changes
```

---

# Step 9 — Trigger the Pipeline

Edit **index.html**.

Add:

```html
<h2>Pipeline Test</h2>
```

Commit changes.

This triggers deployment.

---

# Step 10 — Watch Pipeline Run

Go to:

```
GitHub → Actions
```

You will see:

```
Deploy to S3
```

Wait until:

```
✔ Success
```

---

# Step 11 — Open Your Website 🌍

Go to AWS S3.

Open bucket.

Go to:

```
Properties → Static Website Hosting
```

Copy:

```
Endpoint URL
```

Open in browser.

You should see:

```
Hello from CI/CD 🚀
```

---

# ✅ You Now Built a Real CI/CD Pipeline

You now have:

✔ GitHub repo
✔ Pipeline
✔ AWS deployment
✔ Auto updates

