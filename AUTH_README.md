# Crypto Tracker with Authentication

A full-stack cryptocurrency tracking application with user authentication. View real-time crypto prices and manage your portfolio with a secure account system.

## Features

- 🔐 **User Authentication**: Secure signup/login with JWT and httpOnly cookies
- 📊 **Real-time Crypto Data**: Live cryptocurrency prices from CoinGecko API
- 🔒 **Protected Routes**: Coin details accessible only to authenticated users
- 👤 **User Management**: Account creation, login, logout functionality
- 📈 **Portfolio Tracking**: Track your favorite cryptocurrencies
- 🎨 **Modern UI**: Clean, responsive design with dark theme

## Tech Stack

### Frontend
- React 19.2.0
- React Router 7.10.1
- Vite 7.2.4
- CSS3 (Custom styling)

### Backend
- Node.js with Express
- MongoDB with Mongoose
- JWT for authentication
- bcryptjs for password hashing
- Cookie-based session management

## Prerequisites

Before running this application, make sure you have:

- **Node.js** (v18 or higher)
- **MongoDB** (local installation or MongoDB Atlas account)
- **npm** or **yarn** package manager

## Installation

### 1. Clone or navigate to the project directory

```bash
cd crypto-project2
```

### 2. Install Frontend Dependencies

```bash
npm install
```

### 3. Install Backend Dependencies

```bash
cd server
npm install
cd ..
```

### 4. Configure Environment Variables

The backend uses environment variables for configuration. The `.env` file has already been created in the `server` folder with default values.

**Edit `server/.env` if needed:**

```env
# MongoDB Connection String
MONGODB_URI=mongodb://localhost:27017/crypto-auth

# JWT Secret Key (change this in production!)
JWT_SECRET=crypto-project-jwt-secret-key-2026

# Server Port
PORT=5000

# Node Environment
NODE_ENV=development

# Frontend URL (for CORS)
CLIENT_URL=http://localhost:5173
```

**Important:** If you're using MongoDB Atlas (cloud database), replace the `MONGODB_URI` with your connection string:

```env
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/crypto-auth
```

### 5. Start MongoDB

If using local MongoDB, make sure MongoDB is running:

**Windows:**
```bash
# MongoDB usually runs as a service on Windows
# Check if it's running in Services, or start it manually:
mongod
```

**Mac/Linux:**
```bash
# If installed via Homebrew (Mac):
brew services start mongodb-community

# Or start manually:
mongod --config /usr/local/etc/mongod.conf
```

**MongoDB Atlas:** No need to start anything, just ensure your connection string is correct.

## Running the Application

You need to run both the frontend and backend servers.

### Terminal 1: Start the Backend Server

```bash
cd server
npm run dev
```

You should see:
```
Server running in development mode on port 5000
MongoDB Connected: localhost
```

### Terminal 2: Start the Frontend Development Server

In a new terminal (from the project root):

```bash
npm run dev
```

You should see:
```
  VITE v7.2.4  ready in XXX ms

  ➜  Local:   http://localhost:5173/
```

### 6. Open the Application

Navigate to **[http://localhost:5173](http://localhost:5173)** in your browser.

## Usage

### 1. Create an Account
- Click "Sign Up" on the home page
- Fill in username, email, and password
- Accept terms and conditions
- Click "Sign Up"

### 2. Log In
- Click "Log In" on the home page
- Enter your email and password
- Click "Log In"

### 3. View Coin Details (Protected)
- Once logged in, click on any cryptocurrency card
- View detailed information about that coin
- If you're not logged in, you'll be redirected to the login page

### 4. Log Out
- Click the "Logout" button in the top right corner
- You'll be logged out and redirected to the home page

## API Endpoints

### Authentication Routes

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| POST | `/api/auth/signup` | Create new user account | Public |
| POST | `/api/auth/login` | Login user | Public |
| POST | `/api/auth/logout` | Logout user | Public |
| GET | `/api/auth/verify` | Verify user token | Protected |

### Request/Response Examples

**Signup Request:**
```json
POST /api/auth/signup
{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "secure123"
}
```

**Login Request:**
```json
POST /api/auth/login
{
  "email": "john@example.com",
  "password": "secure123"
}
```

**Success Response:**
```json
{
  "success": true,
  "user": {
    "id": "507f1f77bcf86cd799439011",
    "username": "john_doe",
    "email": "john@example.com"
  }
}
```

## Project Structure

```
crypto-project2/
├── server/                 # Backend application
│   ├── config/
│   │   └── db.js          # MongoDB connection
│   ├── middleware/
│   │   └── auth.js        # JWT authentication middleware
│   ├── models/
│   │   └── User.js        # User schema
│   ├── routes/
│   │   └── auth.js        # Authentication routes
│   ├── .env               # Environment variables
│   ├── .env.example       # Environment template
│   ├── package.json       # Backend dependencies
│   └── server.js          # Express server entry point
│
├── src/                   # Frontend application
│   ├── api/
│   │   ├── auth.js        # Auth API calls
│   │   └── coinGecko.js   # CoinGecko API calls
│   ├── components/
│   │   ├── CryptoCard.jsx
│   │   └── ProtectedRoute.jsx  # Route protection component
│   ├── context/
│   │   └── AuthContext.jsx     # Global auth state
│   ├── pages/
│   │   ├── Home.jsx
│   │   ├── CoinDetail.jsx
│   │   ├── Login.jsx           # Login page
│   │   └── Signup.jsx          # Signup page
│   ├── App.jsx            # App routing
│   ├── main.jsx           # App entry point
│   └── index.css          # Global styles
│
├── public/                # Static assets
├── package.json           # Frontend dependencies
└── README.md             # This file
```

## Security Features

- ✅ **Password Hashing**: Passwords are hashed using bcryptjs before storage
- ✅ **httpOnly Cookies**: JWT tokens stored in httpOnly cookies (not accessible via JavaScript)
- ✅ **CORS Protection**: CORS configured to accept requests only from the frontend
- ✅ **SameSite Cookies**: Protection against CSRF attacks
- ✅ **JWT Expiration**: Tokens expire after 30 days
- ✅ **Input Validation**: Server-side validation for all user inputs

## Troubleshooting

### MongoDB Connection Issues

**Error:** `MongooseServerSelectionError: connect ECONNREFUSED`

**Solution:**
- Ensure MongoDB is running
- Check that `MONGODB_URI` in `.env` is correct
- For local MongoDB: `mongodb://localhost:27017/crypto-auth`
- For Atlas: Ensure your IP is whitelisted

### CORS Errors

**Error:** `Access to fetch at 'http://localhost:5000' from origin 'http://localhost:5173' has been blocked by CORS`

**Solution:**
- Ensure `CLIENT_URL` in `server/.env` matches your frontend URL
- Restart the backend server after changing `.env`

### Cookies Not Being Set

**Problem:** Login/signup succeeds but user is not authenticated

**Solution:**
- Check browser console for cookie warnings
- Ensure `credentials: 'include'` is set in fetch calls (already configured)
- Clear browser cookies and try again
- Check that `sameSite: 'lax'` is appropriate for your setup

### Port Already in Use

**Error:** `EADDRINUSE: address already in use :::5000`

**Solution:**
```bash
# Find and kill the process using port 5000
# Windows:
netstat -ano | findstr :5000
taskkill /PID <PID> /F

# Mac/Linux:
lsof -ti:5000 | xargs kill -9
```

## Development Notes

- Frontend runs on **port 5173** (Vite default)
- Backend runs on **port 5000**
- MongoDB runs on **port 27017** (default)
- Authentication uses httpOnly cookies for security
- Protected routes redirect to `/login` if user is not authenticated

## Future Enhancements

- [ ] Email verification for new accounts
- [ ] Password reset functionality
- [ ] Social authentication (Google, GitHub)
- [ ] User preferences and settings
- [ ] Watchlist/favorites functionality
- [ ] Price alerts
- [ ] Portfolio tracking with investment amounts

## License

MIT

## Support

For issues or questions, please create an issue in the repository.
