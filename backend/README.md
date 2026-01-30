# Learnifyy Backend - Educational Platform API

A comprehensive backend API for an online learning platform built with **Node.js**, **Express.js**, and **MongoDB**. This platform enables users to learn from instructors, purchase courses, track progress, and engage with course content through ratings and reviews.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Key Features](#key-features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Installation & Setup](#installation--setup)
- [Environment Variables](#environment-variables)
- [API Documentation](#api-documentation)
- [Database Models](#database-models)
- [Workflows](#workflows)
- [Middleware](#middleware)
- [Configuration](#configuration)
- [Development](#development)

---

## Project Overview

**Learnifyy** is a full-featured online education platform backend that facilitates:
- User authentication (Students, Instructors, Admins)
- Course creation and management
- Course enrollment and payment processing
- Learning progress tracking
- Rating and review system
- Email notifications
- Cloud-based image/video storage

The backend handles all business logic, database operations, payment processing, and communication with external services like Cloudinary (media storage) and Razorpay (payment gateway).

---

## Key Features

### 1. **Authentication & Authorization**
   - User registration with email verification (OTP)
   - JWT-based authentication
   - Role-based access control (Student, Instructor, Admin)
   - Password reset functionality
   - Secure password hashing with bcrypt

### 2. **Course Management**
   - Create, edit, delete courses (Instructors only)
   - Organize courses with sections and subsections
   - Upload course thumbnails and media
   - Publish/Draft status management
   - Category-based course organization
   - Course search and filtering

### 3. **User Profiles**
   - User profile management
   - Profile image uploads
   - Instructor dashboard
   - Enrolled courses tracking
   - Account deletion

### 4. **Learning Progress**
   - Track student progress through courses
   - Monitor section and subsection completion
   - Update course progress status

### 5. **Payment Integration**
   - Razorpay payment gateway integration
   - Secure payment capture and verification
   - Transaction history

### 6. **Ratings & Reviews**
   - Add ratings and reviews to courses
   - Calculate average course ratings
   - View all reviews for a course

### 7. **Email Notifications**
   - OTP delivery for email verification
   - Course enrollment confirmation emails
   - Password reset emails
   - Email templates for consistency

### 8. **Media Management**
   - Cloudinary integration for image/video uploads
   - Temporary file storage and processing
   - Secure file handling

---

## Tech Stack

| Technology | Purpose |
|------------|---------|
| **Node.js** | Runtime environment |
| **Express.js** | Web framework |
| **MongoDB** | Database |
| **Mongoose** | ODM (Object Data Modeling) |
| **JWT (jsonwebtoken)** | Authentication & Authorization |
| **Bcrypt** | Password hashing |
| **Cloudinary** | Media storage & processing |
| **Razorpay** | Payment processing |
| **Nodemailer** | Email sending |
| **OTP Generator** | Email verification |
| **Express FileUpload** | File upload handling |
| **CORS** | Cross-Origin Resource Sharing |
| **Cookie Parser** | Cookie parsing |
| **Dotenv** | Environment variables |
| **Nodemon** | Development auto-reload |
| **Node Schedule** | Job scheduling |

---

## Project Structure

```
backend/
├── config/                 # Configuration files
│   ├── database.js        # MongoDB connection
│   ├── cloudinary.js      # Cloudinary setup
│   └── rajorpay.js        # Razorpay setup
│
├── controllers/           # Business logic
│   ├── auth.js           # Authentication (signup, login, OTP)
│   ├── category.js       # Course categories
│   ├── course.js         # Course CRUD operations
│   ├── courseProgress.js # Learning progress tracking
│   ├── payments.js       # Payment processing
│   ├── profile.js        # User profile management
│   ├── ratingAndReview.js # Course ratings
│   ├── resetPassword.js  # Password recovery
│   ├── section.js        # Course sections
│   └── subSection.js     # Course subsections
│
├── mail/                  # Email templates
│   └── templates/
│       ├── courseEnrollmentEmail.js
│       ├── emailVerificationTemplate.js
│       └── passwordUpdate.js
│
├── middleware/            # Custom middleware
│   └── auth.js           # JWT validation & role checking
│
├── models/               # Database schemas
│   ├── user.js           # User model
│   ├── course.js         # Course model
│   ├── section.js        # Section model
│   ├── subSection.js     # Subsection model
│   ├── profile.js        # User profile model
│   ├── category.js       # Category model
│   ├── ratingAndReview.js # Review model
│   ├── OTP.js            # OTP model
│   └── courseProgress.js # Progress tracking model
│
├── routes/               # API endpoints
│   ├── user.js           # Auth routes
│   ├── profile.js        # Profile routes
│   ├── course.js         # Course routes
│   └── payments.js       # Payment routes
│
├── utils/                # Utility functions
│   ├── imageUploader.js  # Image upload handler
│   ├── mailSender.js     # Email sending utility
│   └── secToDuration.js  # Time conversion utility
│
├── .env                  # Environment variables
├── package.json          # Dependencies
├── server.js             # Entry point
└── README.md             # This file
```

---

## Installation & Setup

### Prerequisites
- Node.js (v14 or higher)
- MongoDB (local or cloud)
- Cloudinary account
- Razorpay account
- Gmail account (for email sending)

### Steps

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd learnifyy/backend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Create .env file** (see Environment Variables section below)

4. **Start the server**
   ```bash
   # Development mode with auto-reload
   npm run dev
   
   # Production mode
   npm start
   ```

5. **Verify server is running**
   - Visit `http://localhost:5000` (or configured PORT)
   - You should see: "This is Default Route - Everything is OK"

---

## Environment Variables

Create a `.env` file in the backend root directory with the following variables:

```env
# Server
PORT=5000

# Database
MONGODB_URL=mongodb+srv://<username>:<password>@<cluster>.mongodb.net/<database>

# JWT
JWT_SECRET=your_jwt_secret_key_here
JWT_EXPIRE=7d

# Cloudinary
CLOUDINARY_NAME=your_cloudinary_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# Razorpay
RAZORPAY_KEY_ID=your_razorpay_key_id
RAZORPAY_KEY_SECRET=your_razorpay_key_secret

# Email (Gmail)
MAIL_HOST=smtp.gmail.com
MAIL_USER=your_email@gmail.com
MAIL_PASS=your_app_password
MAIL_FROM_EMAIL=your_email@gmail.com

# OTP
OTP_EXPIRE_TIME=5  # minutes

# Frontend URL
FRONTEND_URL=http://localhost:5173
```

---

## API Documentation

### Base URL
```
http://localhost:5000/api/v1
```

### Authentication Routes (`/api/v1/auth`)

#### 1. Send OTP
- **POST** `/sendotp`
- **Body**: `{ email: "user@example.com", accountType: "Student" }`
- **Response**: Sends OTP to email
- **Description**: Generate and send OTP for email verification

#### 2. Signup
- **POST** `/signup`
- **Body**: 
  ```json
  {
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "password": "secure_password",
    "accountType": "Student",
    "otp": "123456",
    "contactNumber": "1234567890"
  }
  ```
- **Response**: User created, JWT token returned
- **Description**: Register new user with email verification

#### 3. Login
- **POST** `/login`
- **Body**: `{ email: "user@example.com", password: "password" }`
- **Response**: JWT token and user details
- **Description**: Authenticate user and return token

#### 4. Change Password
- **POST** `/changepassword`
- **Headers**: `Authorization: Bearer <token>`
- **Body**: `{ oldPassword: "old", newPassword: "new" }`
- **Description**: Update user password (requires authentication)

#### 5. Reset Password Token
- **POST** `/reset-password-token`
- **Body**: `{ email: "user@example.com" }`
- **Response**: Reset token sent to email
- **Description**: Generate password reset token

#### 6. Reset Password
- **POST** `/reset-password`
- **Body**: `{ token: "reset_token", newPassword: "new_password" }`
- **Description**: Reset password using token

---

### Profile Routes (`/api/v1/profile`)

#### 1. Get User Details
- **GET** `/getUserDetails`
- **Headers**: `Authorization: Bearer <token>`
- **Response**: User profile, enrolled courses, progress
- **Description**: Fetch authenticated user's complete profile

#### 2. Update Profile
- **PUT** `/updateProfile`
- **Headers**: `Authorization: Bearer <token>`
- **Body**: 
  ```json
  {
    "firstName": "Jane",
    "lastName": "Smith",
    "about": "Bio text",
    "gender": "Female",
    "dateOfBirth": "1995-01-01",
    "contactNumber": "9876543210"
  }
  ```
- **Description**: Update user profile information

#### 3. Update Profile Image
- **PUT** `/updateUserProfileImage`
- **Headers**: `Authorization: Bearer <token>`
- **Body**: `multipart/form-data` with image file
- **Description**: Upload/update user profile picture

#### 4. Get Enrolled Courses
- **GET** `/getEnrolledCourses`
- **Headers**: `Authorization: Bearer <token>`
- **Response**: List of enrolled courses with progress
- **Description**: Fetch all courses user is enrolled in

#### 5. Instructor Dashboard
- **GET** `/instructorDashboard`
- **Headers**: `Authorization: Bearer <token>` (Instructor only)
- **Response**: Dashboard statistics and course data
- **Description**: Get instructor analytics and course performance

#### 6. Delete Account
- **DELETE** `/deleteProfile`
- **Headers**: `Authorization: Bearer <token>`
- **Body**: `{ password: "user_password" }`
- **Description**: Permanently delete user account

---

### Course Routes (`/api/v1/course`)

#### Course Management

##### 1. Create Course
- **POST** `/createCourse`
- **Headers**: `Authorization: Bearer <token>` (Instructor only)
- **Body**: 
  ```json
  {
    "courseName": "React Basics",
    "courseDescription": "Learn React from scratch",
    "whatYouWillLearn": "Build interactive UIs",
    "category": "category_id",
    "price": 4999,
    "tag": ["web", "react"]
  }
  ```

##### 2. Edit Course
- **PUT** `/editCourse/:courseId`
- **Headers**: `Authorization: Bearer <token>` (Instructor only)
- **Body**: Updated course fields

##### 3. Get Course Details
- **GET** `/getCourseDetails/:courseId`
- **Response**: Basic course info
- **Description**: Get limited course information

##### 4. Get Full Course Details
- **GET** `/getFullCourseDetails/:courseId`
- **Response**: Complete course with all sections and subsections
- **Description**: Detailed course view

##### 5. Get All Courses
- **GET** `/getAllCourses`
- **Response**: All published courses
- **Description**: Browse all available courses

##### 6. Get Instructor Courses
- **GET** `/getInstructorCourses`
- **Headers**: `Authorization: Bearer <token>` (Instructor only)
- **Response**: All courses created by instructor

##### 7. Delete Course
- **DELETE** `/deleteCourse/:courseId`
- **Headers**: `Authorization: Bearer <token>` (Instructor only)

---

#### Section Management

##### 1. Add Section
- **POST** `/addSection`
- **Headers**: `Authorization: Bearer <token>` (Instructor only)
- **Body**: `{ sectionName: "Module 1", courseId: "course_id" }`

##### 2. Update Section
- **POST** `/updateSection`
- **Headers**: `Authorization: Bearer <token>` (Instructor only)
- **Body**: `{ sectionName: "Updated Name", sectionId: "section_id" }`

##### 3. Delete Section
- **POST** `/deleteSection`
- **Headers**: `Authorization: Bearer <token>` (Instructor only)
- **Body**: `{ sectionId: "section_id" }`

---

#### Subsection Management

##### 1. Add Subsection
- **POST** `/addSubSection`
- **Headers**: `Authorization: Bearer <token>` (Instructor only)
- **Body**: 
  ```json
  {
    "sectionId": "section_id",
    "title": "Lecture 1",
    "description": "Introduction",
    "videoUrl": "video_file"
  }
  ```

##### 2. Update Subsection
- **POST** `/updateSubSection`
- **Headers**: `Authorization: Bearer <token>` (Instructor only)

##### 3. Delete Subsection
- **POST** `/deleteSubSection`
- **Headers**: `Authorization: Bearer <token>` (Instructor only)

---

#### Category Management

##### 1. Create Category
- **POST** `/createCategory`
- **Headers**: `Authorization: Bearer <token>` (Admin only)
- **Body**: `{ name: "Programming", description: "..." }`

##### 2. Show All Categories
- **GET** `/showAllCategories`
- **Response**: All course categories

##### 3. Get Category Page Details
- **GET** `/getCategoryPageDetails/:categoryId`
- **Response**: Courses in category with details

---

#### Course Progress & Enrollment

##### 1. Update Course Progress
- **POST** `/updateCourseProgress/:courseId/:subSectionId`
- **Headers**: `Authorization: Bearer <token>` (Student only)
- **Description**: Mark subsection as completed

---

#### Ratings & Reviews

##### 1. Create Rating/Review
- **POST** `/createRating`
- **Headers**: `Authorization: Bearer <token>` (Student only)
- **Body**: 
  ```json
  {
    "rating": 4,
    "review": "Great course!",
    "courseId": "course_id"
  }
  ```

##### 2. Get Average Rating
- **GET** `/getAverageRating/:courseId`
- **Response**: Average rating for course

##### 3. Get All Ratings
- **GET** `/getAllRatingReview/:courseId`
- **Response**: All reviews for course

---

### Payment Routes (`/api/v1/payment`)

#### 1. Capture Payment
- **POST** `/capturePayment`
- **Headers**: `Authorization: Bearer <token>` (Student only)
- **Body**: 
  ```json
  {
    "courses": ["course_id_1", "course_id_2"],
    "amount": 9998
  }
  ```
- **Response**: Razorpay order ID
- **Description**: Initiate payment capture

#### 2. Verify Payment
- **POST** `/verifyPayment`
- **Headers**: `Authorization: Bearer <token>` (Student only)
- **Body**: 
  ```json
  {
    "razorpay_order_id": "order_id",
    "razorpay_payment_id": "payment_id",
    "razorpay_signature": "signature",
    "courses": ["course_id"]
  }
  ```
- **Response**: Enrollment confirmation, enrollment email sent
- **Description**: Verify and complete payment, enroll in course(s)

---

## Database Models

### 1. User Model
```javascript
{
  firstName: String,
  lastName: String,
  email: String (unique),
  password: String (hashed),
  accountType: Enum ['Admin', 'Instructor', 'Student'],
  active: Boolean,
  approved: Boolean,
  additionalDetails: ObjectId (ref: Profile),
  courses: [ObjectId] (ref: Course),
  image: String (URL),
  token: String (JWT),
  resetPasswordTokenExpires: Date,
  courseProgress: [ObjectId] (ref: CourseProgress),
  timestamps: true
}
```

### 2. Course Model
```javascript
{
  courseName: String,
  courseDescription: String,
  instructor: ObjectId (ref: User),
  whatYouWillLearn: String,
  courseContent: [ObjectId] (ref: Section),
  ratingAndReviews: [ObjectId] (ref: RatingAndReview),
  price: Number,
  thumbnail: String (Cloudinary URL),
  category: ObjectId (ref: Category),
  tag: [String],
  studentsEnrolled: [ObjectId] (ref: User),
  instructions: [String],
  status: Enum ['Draft', 'Published'],
  createdAt: Date,
  updatedAt: Date
}
```

### 3. Section Model
```javascript
{
  sectionName: String,
  course: ObjectId (ref: Course),
  subSection: [ObjectId] (ref: SubSection),
  timestamps: true
}
```

### 4. SubSection Model
```javascript
{
  title: String,
  timeDuration: String,
  description: String,
  videoUrl: String (Cloudinary URL),
  section: ObjectId (ref: Section),
  timestamps: true
}
```

### 5. Profile Model
```javascript
{
  user: ObjectId (ref: User),
  gender: String,
  dateOfBirth: Date,
  about: String,
  contactNumber: String,
  timestamps: true
}
```

### 6. Category Model
```javascript
{
  name: String,
  description: String,
  courses: [ObjectId] (ref: Course),
  timestamps: true
}
```

### 7. RatingAndReview Model
```javascript
{
  user: ObjectId (ref: User),
  rating: Number (1-5),
  review: String,
  course: ObjectId (ref: Course),
  timestamps: true
}
```

### 8. OTP Model
```javascript
{
  email: String,
  otp: String,
  createdAt: Date (with TTL index for auto-deletion)
}
```

### 9. CourseProgress Model
```javascript
{
  courseId: ObjectId (ref: Course),
  userId: ObjectId (ref: User),
  completedVideos: [ObjectId] (ref: SubSection),
  timestamps: true
}
```

---

## Workflows

### 1. **User Registration Workflow**
```
User requests OTP
    ↓
Server validates email, generates OTP
    ↓
OTP sent to email via Nodemailer
    ↓
User submits signup with OTP
    ↓
OTP verified against database
    ↓
User profile created
    ↓
Password hashed with bcrypt
    ↓
JWT token issued
    ↓
User logged in and redirected
```

### 2. **Course Creation Workflow**
```
Instructor creates course
    ↓
Course saved as Draft
    ↓
Instructor adds sections
    ↓
Instructor adds subsections with videos
    ↓
Videos uploaded to Cloudinary
    ↓
Instructor publishes course
    ↓
Course visible to students
    ↓
Course appears in category listings
```

### 3. **Course Enrollment & Payment Workflow**
```
Student views course
    ↓
Student clicks "Enroll" button
    ↓
Initiates payment capture
    ↓
Razorpay order created
    ↓
Student completes payment
    ↓
Razorpay returns payment details
    ↓
Server verifies payment signature
    ↓
Student enrolled in course
    ↓
CourseProgress document created
    ↓
Enrollment email sent to student
    ↓
Course appears in "My Courses"
```

### 4. **Learning Progress Workflow**
```
Student starts course
    ↓
Student watches subsection video
    ↓
Subsection completion marked
    ↓
CourseProgress updated
    ↓
Completion percentage calculated
    ↓
Student moves to next subsection
    ↓
Progress tracked in dashboard
```

### 5. **Rating & Review Workflow**
```
Student completes course
    ↓
Student submits rating and review
    ↓
Rating stored in RatingAndReview
    ↓
Rating linked to course
    ↓
Average rating recalculated
    ↓
Rating visible to other students
```

### 6. **Password Reset Workflow**
```
User clicks "Forgot Password"
    ↓
User enters email
    ↓
Server generates reset token
    ↓
Token expiration set (typically 24 hours)
    ↓
Reset link sent to email
    ↓
User clicks link and enters new password
    ↓
Server verifies token
    ↓
Password updated and hashed
    ↓
Token invalidated
```

### 7. **Image Upload Workflow**
```
User selects image
    ↓
Express fileUpload stores in /tmp
    ↓
Image sent to Cloudinary
    ↓
Cloudinary returns secure URL
    ↓
Temp file deleted
    ↓
URL stored in database
    ↓
Image displayed in UI
```

---

## Middleware

### Auth Middleware (`middleware/auth.js`)

#### `auth` - General Authentication
- **Purpose**: Verify JWT token and extract user information
- **Usage**: Protects all user-specific routes
- **Flow**:
  1. Extract token from request (body, cookies, or headers)
  2. Verify JWT signature
  3. Decode token and attach user to `req.user`
  4. Allow request to proceed

#### `isStudent` - Student Role Check
- **Purpose**: Verify user is a Student
- **Usage**: Restricts routes to students only
- **Example**: Payment routes

#### `isInstructor` - Instructor Role Check
- **Purpose**: Verify user is an Instructor
- **Usage**: Course creation and management routes
- **Example**: Create course, add sections

#### `isAdmin` - Admin Role Check
- **Purpose**: Verify user is an Admin
- **Usage**: Administrative routes
- **Example**: Category creation

### CORS Middleware
- **Configuration**: Allows requests from all origins
- **Includes**: Credentials support

### File Upload Middleware
- **Purpose**: Handle multipart file uploads
- **Temporary Storage**: `/tmp` directory
- **Usage**: Course thumbnails, profile images, videos

---

## Configuration

### Database Configuration (`config/database.js`)
```javascript
// Connects to MongoDB using Mongoose
// Sets up connection pool and error handling
// Exports: connectDB()
```

### Cloudinary Configuration (`config/cloudinary.js`)
```javascript
// Initializes Cloudinary with API credentials
// Configures media upload settings
// Exports: cloudinaryConnect()
```

### Razorpay Configuration (`config/rajorpay.js`)
```javascript
// Initializes Razorpay instance with API keys
// Provides payment gateway functionality
// Exports: Razorpay instance
```

---

## Development

### Running in Development Mode
```bash
npm run dev
# Uses nodemon for auto-reload on file changes
```

### Running in Production
```bash
npm start
# Standard Node.js execution
```

### Build Project
```bash
npm run build
# Installs dependencies
```

### Available Scripts
- `npm run dev` - Development server with hot reload
- `npm start` - Production server
- `npm run build` - Install dependencies

---

## Security Best Practices

1. **Password Security**
   - All passwords hashed with bcrypt
   - Salt rounds: 10+
   - Never store plain text passwords

2. **JWT Tokens**
   - Tokens expire after 7 days
   - Secret key stored in environment variables
   - Include role information in token

3. **File Uploads**
   - Temporary files stored in `/tmp`
   - Files uploaded to Cloudinary (external storage)
   - Original files deleted after processing

4. **Payment Security**
   - Razorpay signature verification
   - Amount verification before processing
   - No sensitive data in requests/responses

5. **Email Verification**
   - OTP sent to email before signup
   - OTP expires after 5 minutes
   - OTP used only once

6. **CORS & HTTPS**
   - CORS properly configured
   - Should use HTTPS in production
   - Credentials included in cross-origin requests

---

## Error Handling

The API returns structured error responses:

```json
{
  "success": false,
  "message": "Error description",
  "error": "Detailed error message"
}
```

Common HTTP Status Codes:
- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized (missing/invalid token)
- `403` - Forbidden (insufficient permissions)
- `404` - Not Found
- `500` - Server Error

---

## Future Enhancements

- [ ] Video streaming optimization
- [ ] Course certificates generation
- [ ] Discussion forums
- [ ] Live streaming support
- [ ] Mobile app API optimization
- [ ] Admin dashboard enhancements
- [ ] Advanced analytics
- [ ] Wishlist/Bookmark features
- [ ] Coupons and discounts
- [ ] Course prerequisites validation

---

## Support & Documentation

For more information or issues, please refer to:
- Express.js Documentation: https://expressjs.com/
- MongoDB Documentation: https://docs.mongodb.com/
- JWT Documentation: https://jwt.io/
- Cloudinary Documentation: https://cloudinary.com/documentation/
- Razorpay Documentation: https://razorpay.com/docs/

---

## License

ISC License - See package.json for details

---

## Author

Created and maintained by the Learnifyy Development Team

---

**Last Updated**: January 26, 2026
