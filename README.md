# mern-week10


### 1. MongoDB Setup
I used the MongoDB Atlas URL provided in the assignment:

```
mongodb+srv://test:qazqwe123@mongodb.txkjsso.mongodb.net/blog-app
```

---

### 2. S3 Buckets

#### a. Frontend Bucket
- Name: `huda-frontend`
- Region: `eu-north-1`
- Enabled static website hosting.
- Added a bucket policy to allow public access.

#### b. Media Bucket
- Name: `huda-media`
- Region: `eu-north-1`
- Added a CORS policy to allow image uploads from the app.

---

### 3. IAM User
- Created IAM user `blog-app-user`
- Gave access only to the `huda-media` bucket using a custom policy
- Used the access key and secret key in the backend `.env` file
- Deleted the IAM user after deployment (as required)

---

### 4. EC2 Instance
- Launched an Ubuntu 22.04 EC2 instance (t2.micro)
- Opened ports: 22 (SSH), 80 (HTTP), 5000 (for backend API)
- Installed Node.js, npm, and pm2
- Cloned the project:

```
git clone https://github.com/cw-barry/blog-app-MERN.git /home/ubuntu/blog-app
```

- Created `.env` in `/backend` with MongoDB, S3, and JWT settings
- Started the backend using pm2:

```
cd backend
npm install
pm2 start index.js --name blog-backend
pm2 save
```

---

### 5. Frontend Deployment

- Created `.env` in `/frontend`:

```
VITE_BASE_URL=http://<my-ec2-ip>:5000/api
VITE_MEDIA_BASE_URL=https://huda-media.eu-north-1.amazonaws.com
```

- Installed dependencies and built the frontend:

```
pnpm install
pnpm run build
```

- Uploaded the `dist/` folder to the frontend S3 bucket:

```
aws s3 sync dist/ s3://huda-frontend/
```

