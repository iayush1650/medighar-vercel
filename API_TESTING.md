# 🧪 MediGhar API Testing Guide

Quick reference for testing all API endpoints.

## Base URL
```
http://localhost:5000/api
```

## 🔐 Authentication Endpoints

### 1. Register User
```bash
POST /auth/register

Body:
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123",
  "phone": "9876543210"
}
```

### 2. Login
```bash
POST /auth/login

Body:
{
  "email": "john@example.com",
  "password": "password123"
}

Response: Save the "token" for authenticated requests
```

### 3. Get Profile (Requires Auth)
```bash
GET /auth/profile

Headers:
Authorization: Bearer YOUR_TOKEN_HERE
```

## 📧 Contact Endpoints

### Submit Contact Form
```bash
POST /contact

Body:
{
  "name": "Jane Smith",
  "email": "jane@example.com",
  "inquiryType": "General Inquiry",
  "message": "I would like to know more about your services."
}
```

### Get All Contacts (Admin Only)
```bash
GET /contact?status=new&page=1&limit=20

Headers:
Authorization: Bearer ADMIN_TOKEN
```

## 💼 Career Endpoints

### Submit Application (with file)
```bash
POST /careers/apply

Headers:
Content-Type: multipart/form-data

Form Data:
- name: "Alice Johnson"
- email: "alice@example.com"
- phone: "9876543210"
- position: "Full Stack Internship"
- resume: [FILE]
- coverLetter: "I am interested in..."
```

### Get All Applications (Admin Only)
```bash
GET /careers/applications?status=new&page=1

Headers:
Authorization: Bearer ADMIN_TOKEN
```

## 👨‍⚕️ Doctor Endpoints

### Get All Doctors
```bash
GET /doctors?specialization=Cardiologist&minRating=4&page=1
```

### Get Doctor Details
```bash
GET /doctors/:doctorId
```

### Add Doctor (Admin Only)
```bash
POST /doctors

Headers:
Authorization: Bearer ADMIN_TOKEN

Body:
{
  "name": "Dr. Sarah Johnson",
  "email": "dr.sarah@example.com",
  "phone": "9876543210",
  "specialization": ["Cardiologist"],
  "experience": 10,
  "registrationNumber": "MED123456",
  "consultationFee": 500,
  "qualifications": [{
    "degree": "MBBS",
    "institution": "Medical College",
    "year": 2010
  }],
  "availability": {
    "monday": {
      "available": true,
      "slots": ["09:00-10:00", "10:00-11:00", "11:00-12:00"]
    }
  }
}
```

### Book Consultation (Requires Auth)
```bash
POST /doctors/consultations

Headers:
Authorization: Bearer YOUR_TOKEN

Body:
{
  "doctorId": "DOCTOR_ID",
  "scheduledDate": "2024-02-01T10:00:00Z",
  "timeSlot": "10:00-11:00",
  "consultationType": "video",
  "symptoms": "Chest pain and shortness of breath"
}
```

## 🏥 Pharmacy Endpoints

### Get All Pharmacies
```bash
GET /pharmacies?city=Delhi&deliveryAvailable=true
```

### Get Nearby Pharmacies (Geospatial)
```bash
GET /pharmacies/nearby?longitude=77.2090&latitude=28.6139&maxDistance=5000
```

### Add Pharmacy (Admin Only)
```bash
POST /pharmacies

Headers:
Authorization: Bearer ADMIN_TOKEN

Body:
{
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
  "licenseNumber": "PHARM123456",
  "deliveryAvailable": true,
  "deliveryRadius": 5,
  "deliveryFee": 50,
  "workingHours": {
    "monday": {
      "open": "09:00",
      "close": "21:00",
      "isOpen": true
    }
  }
}
```

## 💊 Medicine Endpoints

### Search Medicines
```bash
GET /medicines?search=paracetamol&category=Pain Relief&page=1
```

### Get Medicine Details
```bash
GET /medicines/:medicineId
```

### Compare Prices (Your Unique Feature!)
```bash
GET /medicines/:medicineId/prices
```

### Add Medicine (Admin Only)
```bash
POST /medicines

Headers:
Authorization: Bearer ADMIN_TOKEN

Body:
{
  "name": "Paracetamol",
  "genericName": "Acetaminophen",
  "manufacturer": "PharmaCo",
  "category": "Pain Relief",
  "dosageForm": "Tablet",
  "strength": "500mg",
  "packSize": "10 tablets",
  "requiresPrescription": false,
  "pricing": [{
    "pharmacy": "PHARMACY_ID",
    "price": 50,
    "discount": 10,
    "inStock": true,
    "stockQuantity": 100
  }]
}
```

### Update Medicine Pricing
```bash
PUT /medicines/:medicineId/pricing

Headers:
Authorization: Bearer ADMIN_TOKEN

Body:
{
  "pharmacyId": "PHARMACY_ID",
  "price": 45,
  "discount": 15,
  "inStock": true,
  "stockQuantity": 150
}
```

## 🛒 Order Endpoints

### Create Order (Requires Auth)
```bash
POST /orders

Headers:
Authorization: Bearer YOUR_TOKEN
Content-Type: multipart/form-data

Form Data:
- pharmacyId: "PHARMACY_ID"
- items: [{"medicineId": "MED_ID", "quantity": 2}]
- deliveryAddress: {"name": "John", "phone": "9876543210", "street": "123 Main St", "city": "Delhi", "state": "Delhi", "pincode": "110001"}
- paymentMethod: "cash-on-delivery"
- prescription: [FILE] (if required)
```

### Get User Orders (Requires Auth)
```bash
GET /orders?status=pending&page=1

Headers:
Authorization: Bearer YOUR_TOKEN
```

### Get Order Details (Requires Auth)
```bash
GET /orders/:orderId

Headers:
Authorization: Bearer YOUR_TOKEN
```

### Update Order Status (Admin/Pharmacy)
```bash
PUT /orders/:orderId/status

Headers:
Authorization: Bearer ADMIN_TOKEN

Body:
{
  "status": "confirmed",
  "note": "Order confirmed and being prepared"
}
```

### Cancel Order (Requires Auth)
```bash
PUT /orders/:orderId/cancel

Headers:
Authorization: Bearer YOUR_TOKEN

Body:
{
  "reason": "Changed my mind"
}
```

## 📝 Response Format

### Success Response
```json
{
  "success": true,
  "message": "Optional message",
  "data": { ... },
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "pages": 5
  }
}
```

### Error Response
```json
{
  "success": false,
  "message": "Error message",
  "errors": [
    {
      "field": "email",
      "message": "Please provide a valid email"
    }
  ]
}
```

## 🔑 Getting Admin Access

After registering a user, connect to MongoDB and update the role:

```bash
# Using mongosh
mongosh
use medighar
db.users.updateOne(
  { email: "admin@example.com" },
  { $set: { role: "admin" } }
)
```

## 🎯 Testing Workflow

1. **Register a user** → Save the token
2. **Test contact form** (public)
3. **Test career application** (public, with file)
4. **Create admin user** → Get admin token
5. **Add doctors** (admin)
6. **Add pharmacies** (admin)
7. **Add medicines with pricing** (admin)
8. **Search medicines** (public)
9. **Compare prices** (public)
10. **Book consultation** (authenticated user)
11. **Create order** (authenticated user)
12. **Track order** (authenticated user)

## 🛠️ Tools

**Recommended:**
- **Postman** - https://www.postman.com/
- **Thunder Client** (VS Code Extension)
- **cURL** (Command line)

**Browser Testing:**
- Health check: http://localhost:5000/api/health
- Welcome: http://localhost:5000/

---

**Happy Testing! 🚀**
