# 🚀 MediGhar Backend - Quick Start Guide

Get your MediGhar backend up and running in 5 minutes!

## Step 1: Install Dependencies

```bash
cd backend
npm install
```

## Step 2: Set Up Environment Variables

Create a `.env` file:

```bash
cp .env.example .env
```

Edit `.env` with your settings:

```env
PORT=5000
NODE_ENV=development
MONGODB_URI=mongodb://localhost:27017/medighar
JWT_SECRET=medighar_secret_key_2024
JWT_EXPIRE=7d
FRONTEND_URL=http://localhost:3000
```

## Step 3: Start MongoDB

**Windows:**
```bash
net start MongoDB
```

**macOS/Linux:**
```bash
sudo systemctl start mongod
```

**Or use MongoDB Atlas (Cloud):**
1. Create account at https://www.mongodb.com/cloud/atlas
2. Create a cluster
3. Get connection string
4. Update `MONGODB_URI` in `.env`

## Step 4: Start the Server

**Development mode (with auto-reload):**
```bash
npm run dev
```

**Production mode:**
```bash
npm start
```

You should see:

```
╔═══════════════════════════════════════════════════════════╗
║                                                           ║
║   🏥  MediGhar Backend API Server                        ║
║                                                           ║
║   🚀  Server running on port 5000                        ║
║   🌍  Environment: development                           ║
║   📡  API Base URL: http://localhost:5000/api           ║
║                                                           ║
╚═══════════════════════════════════════════════════════════╝
```

## Step 5: Test the API

**Check health:**
```bash
curl http://localhost:5000/api/health
```

**Register a user:**
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test User",
    "email": "test@example.com",
    "password": "password123",
    "phone": "9876543210"
  }'
```

**Submit contact form:**
```bash
curl -X POST http://localhost:5000/api/contact \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "inquiryType": "General Inquiry",
    "message": "Test message"
  }'
```

## 🎯 Common Tasks

### Create an Admin User

1. Register a normal user first
2. Connect to MongoDB:
   ```bash
   mongosh
   use medighar
   ```
3. Update user role:
   ```javascript
   db.users.updateOne(
     { email: "admin@example.com" },
     { $set: { role: "admin" } }
   )
   ```

### Add Sample Doctor

```bash
# First, login as admin and get the token
# Then use the token in Authorization header

curl -X POST http://localhost:5000/api/doctors \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -d '{
    "name": "Dr. Sarah Johnson",
    "email": "dr.sarah@example.com",
    "phone": "9876543210",
    "specialization": ["General Physician"],
    "experience": 10,
    "registrationNumber": "MED123456",
    "consultationFee": 500,
    "qualifications": [{
      "degree": "MBBS",
      "institution": "Medical College",
      "year": 2010
    }]
  }'
```

### Add Sample Pharmacy

```bash
curl -X POST http://localhost:5000/api/pharmacies \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -d '{
    "name": "City Pharmacy",
    "email": "city@pharmacy.com",
    "phone": "9876543210",
    "location": {
      "type": "Point",
      "coordinates": [77.2090, 28.6139]
    },
    "address": {
      "street": "123 Main Street",
      "city": "Delhi",
      "state": "Delhi",
      "pincode": "110001"
    },
    "licenseNumber": "PHARM123456"
  }'
```

### Add Sample Medicine

```bash
curl -X POST http://localhost:5000/api/medicines \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -d '{
    "name": "Paracetamol",
    "genericName": "Acetaminophen",
    "manufacturer": "PharmaCo",
    "category": "Pain Relief",
    "dosageForm": "Tablet",
    "strength": "500mg",
    "packSize": "10 tablets",
    "requiresPrescription": false,
    "pricing": [{
      "pharmacy": "PHARMACY_ID_HERE",
      "price": 50,
      "discount": 10,
      "inStock": true,
      "stockQuantity": 100
    }]
  }'
```

## 🔧 Troubleshooting

### MongoDB Connection Error

**Error:** `MongoNetworkError: connect ECONNREFUSED`

**Solution:**
- Make sure MongoDB is running
- Check if the connection string in `.env` is correct
- For Windows: Run `net start MongoDB` as administrator

### Port Already in Use

**Error:** `Error: listen EADDRINUSE: address already in use :::5000`

**Solution:**
- Change the PORT in `.env` to a different number (e.g., 5001)
- Or kill the process using port 5000

### JWT Secret Error

**Error:** `secretOrPrivateKey must have a value`

**Solution:**
- Make sure `JWT_SECRET` is set in your `.env` file
- Restart the server after updating `.env`

## 📱 Connect Frontend

Update your frontend to use the backend API:

```javascript
// In your frontend code
const API_URL = 'http://localhost:5000/api';

// Example: Submit contact form
fetch(`${API_URL}/contact`, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: 'John Doe',
    email: 'john@example.com',
    inquiryType: 'General Inquiry',
    message: 'Hello!'
  })
})
.then(res => res.json())
.then(data => console.log(data));
```

## 🎉 Next Steps

1. ✅ Test all API endpoints using Postman or cURL
2. ✅ Create admin user and add sample data
3. ✅ Connect your frontend to the backend
4. ✅ Test contact form integration
5. ✅ Test career application with file upload
6. ✅ Implement authentication in frontend
7. ✅ Test medicine price comparison feature

## 📚 Resources

- [Full API Documentation](./README.md)
- [MongoDB Documentation](https://docs.mongodb.com/)
- [Express.js Guide](https://expressjs.com/)
- [JWT Introduction](https://jwt.io/introduction)

---

**Need help?** Contact: medighar151@gmail.com
