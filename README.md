# 🏥 MediGhar Backend API

Professional backend API for the MediGhar Healthcare Platform built with Node.js, Express, and MongoDB.

## 🚀 Features

- **User Authentication** - JWT-based authentication with role-based access control
- **Contact Management** - Handle contact form submissions
- **Career Applications** - Job application system with resume uploads
- **Doctor Management** - CRUD operations for doctors with consultation booking
- **Pharmacy Management** - Pharmacy listings with geospatial nearby search
- **Medicine Management** - Medicine catalog with price comparison across pharmacies
- **Order Processing** - Complete order management with status tracking
- **File Uploads** - Support for resumes, prescriptions, and profile images
- **Security** - Helmet, CORS, rate limiting, input validation
- **Error Handling** - Centralized error handling with proper status codes

## 📋 Prerequisites

- Node.js (v14 or higher)
- MongoDB (v4.4 or higher)
- npm or yarn

## 🛠️ Installation

1. **Clone the repository**
   ```bash
   cd backend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure environment variables**
   
   Create a `.env` file in the backend directory:
   ```bash
   cp .env.example .env
   ```

   Update the `.env` file with your configuration:
   ```env
   PORT=5000
   NODE_ENV=development
   MONGODB_URI=mongodb://localhost:27017/medighar
   JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
   JWT_EXPIRE=7d
   FRONTEND_URL=http://localhost:3000
   ```

4. **Start MongoDB**
   
   Make sure MongoDB is running on your system:
   ```bash
   # Windows
   net start MongoDB
   
   # macOS/Linux
   sudo systemctl start mongod
   ```

5. **Run the server**
   
   Development mode (with auto-reload):
   ```bash
   npm run dev
   ```

   Production mode:
   ```bash
   npm start
   ```

6. **Verify the server is running**
   
   Open your browser and navigate to:
   ```
   http://localhost:5000/api/health
   ```

## 📁 Project Structure

```
backend/
├── src/
│   ├── config/
│   │   └── database.js          # MongoDB connection
│   ├── models/                  # Mongoose models
│   │   ├── User.js
│   │   ├── Doctor.js
│   │   ├── Pharmacy.js
│   │   ├── Medicine.js
│   │   ├── Order.js
│   │   ├── Consultation.js
│   │   ├── Contact.js
│   │   └── Career.js
│   ├── middleware/              # Custom middleware
│   │   ├── auth.js
│   │   ├── validation.js
│   │   └── errorHandler.js
│   ├── routes/                  # API routes
│   │   ├── auth.js
│   │   ├── contact.js
│   │   ├── careers.js
│   │   ├── doctors.js
│   │   ├── pharmacies.js
│   │   ├── medicines.js
│   │   └── orders.js
│   ├── controllers/             # Route controllers
│   │   ├── authController.js
│   │   ├── contactController.js
│   │   ├── careerController.js
│   │   ├── doctorController.js
│   │   ├── pharmacyController.js
│   │   ├── medicineController.js
│   │   └── orderController.js
│   ├── utils/                   # Utility functions
│   │   ├── fileUpload.js
│   │   └── asyncHandler.js
│   └── server.js                # Main server file
├── uploads/                     # Uploaded files
├── .env.example                 # Environment variables template
├── .gitignore
├── package.json
└── README.md
```

## 🔐 API Endpoints

### Authentication (`/api/auth`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| POST | `/register` | Register new user | Public |
| POST | `/login` | Login user | Public |
| GET | `/profile` | Get user profile | Private |
| PUT | `/profile` | Update user profile | Private |
| PUT | `/change-password` | Change password | Private |

### Contact (`/api/contact`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| POST | `/` | Submit contact form | Public |
| GET | `/` | Get all submissions | Admin |
| GET | `/:id` | Get single submission | Admin |
| PUT | `/:id/status` | Update status | Admin |
| DELETE | `/:id` | Delete submission | Admin |

### Careers (`/api/careers`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| POST | `/apply` | Submit application | Public |
| GET | `/applications` | Get all applications | Admin |
| GET | `/applications/:id` | Get single application | Admin |
| PUT | `/applications/:id/status` | Update status | Admin |
| DELETE | `/applications/:id` | Delete application | Admin |

### Doctors (`/api/doctors`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| GET | `/` | Get all doctors | Public |
| GET | `/:id` | Get doctor details | Public |
| POST | `/` | Add new doctor | Admin |
| PUT | `/:id` | Update doctor | Admin |
| DELETE | `/:id` | Delete doctor | Admin |
| POST | `/consultations` | Book consultation | Private |
| GET | `/consultations/my` | Get user consultations | Private |
| PUT | `/consultations/:id/cancel` | Cancel consultation | Private |

### Pharmacies (`/api/pharmacies`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| GET | `/` | Get all pharmacies | Public |
| GET | `/nearby` | Get nearby pharmacies | Public |
| GET | `/:id` | Get pharmacy details | Public |
| POST | `/` | Add new pharmacy | Admin |
| PUT | `/:id` | Update pharmacy | Admin |
| DELETE | `/:id` | Delete pharmacy | Admin |

### Medicines (`/api/medicines`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| GET | `/` | Search medicines | Public |
| GET | `/:id` | Get medicine details | Public |
| GET | `/:id/prices` | Compare prices | Public |
| POST | `/` | Add new medicine | Admin |
| PUT | `/:id` | Update medicine | Admin |
| PUT | `/:id/pricing` | Update pricing | Admin/Pharmacy |
| DELETE | `/:id` | Delete medicine | Admin |

### Orders (`/api/orders`)

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| POST | `/` | Create order | Private |
| GET | `/` | Get user orders | Private |
| GET | `/all` | Get all orders | Admin/Pharmacy |
| GET | `/:id` | Get order details | Private |
| PUT | `/:id/status` | Update order status | Admin/Pharmacy |
| PUT | `/:id/cancel` | Cancel order | Private |

## 🧪 Testing the API

### Using cURL

**Register a new user:**
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "phone": "9876543210"
  }'
```

**Login:**
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "password123"
  }'
```

**Submit contact form:**
```bash
curl -X POST http://localhost:5000/api/contact \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Jane Smith",
    "email": "jane@example.com",
    "inquiryType": "General Inquiry",
    "message": "I would like to know more about your services."
  }'
```

### Using Postman

1. Import the API endpoints into Postman
2. Set the base URL: `http://localhost:5000/api`
3. For protected routes, add the JWT token in the Authorization header:
   - Type: Bearer Token
   - Token: `<your_jwt_token>`

## 🔒 Authentication

The API uses JWT (JSON Web Tokens) for authentication. After logging in, you'll receive a token that must be included in the Authorization header for protected routes:

```
Authorization: Bearer <your_jwt_token>
```

## 👥 User Roles

- **user** - Regular users (default)
- **admin** - Administrators with full access
- **pharmacy** - Pharmacy owners with limited admin access

## 📝 Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| PORT | Server port | 5000 |
| NODE_ENV | Environment (development/production) | development |
| MONGODB_URI | MongoDB connection string | mongodb://localhost:27017/medighar |
| JWT_SECRET | Secret key for JWT | - |
| JWT_EXPIRE | JWT expiration time | 7d |
| FRONTEND_URL | Frontend URL for CORS | http://localhost:3000 |
| MAX_FILE_SIZE | Maximum file upload size | 5242880 (5MB) |

## 🚨 Error Handling

The API uses consistent error responses:

```json
{
  "success": false,
  "message": "Error message here",
  "errors": [] // Optional validation errors
}
```

## 📊 Response Format

All successful responses follow this format:

```json
{
  "success": true,
  "message": "Optional message",
  "data": {}, // Response data
  "pagination": {} // Optional pagination info
}
```

## 🔧 Development

**Install nodemon for auto-reload:**
```bash
npm install -D nodemon
```

**Run in development mode:**
```bash
npm run dev
```

## 📦 Production Deployment

1. Set `NODE_ENV=production` in your `.env` file
2. Use a production MongoDB instance (MongoDB Atlas recommended)
3. Set a strong `JWT_SECRET`
4. Configure proper CORS settings
5. Use a process manager like PM2:

```bash
npm install -g pm2
pm2 start src/server.js --name medighar-api
pm2 save
pm2 startup
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

## 📄 License

ISC

## 👨‍💻 Support

For support, email medighar151@gmail.com or visit our website.

---

**Made with ❤️ by the MediGhar Team**
