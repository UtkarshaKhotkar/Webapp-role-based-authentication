# Implementation Plan

- [ ] 1. Initialize project structure and dependencies
  - Create root directory with backend and frontend folders
  - Initialize backend with Express, TypeScript, Prisma, and authentication dependencies
  - Initialize frontend with Next.js 14+, TypeScript, TailwindCSS, and form libraries
  - Create .env.example files for both backend and frontend
  - _Requirements: 7.3, 8.1_

- [ ] 2. Set up backend database and Prisma
  - Create Prisma schema with User model including id, email, name, password, role, and timestamps
  - Define Role enum with USER and ADMIN values
  - Add unique constraint and index on email field
  - Generate Prisma client
  - _Requirements: 5.1, 5.2, 5.3, 5.4_

- [ ] 3. Implement backend authentication utilities
  - [ ] 3.1 Create password hashing utility using bcrypt with 10 salt rounds
    - Write hashPassword function
    - Write comparePassword function
    - _Requirements: 1.3, 5.5_
  
  - [ ] 3.2 Create JWT utility functions
    - Write generateToken function that includes userId, email, name, and role in payload
    - Write verifyToken function to decode and validate JWT
    - Set token expiration to 7 days
    - _Requirements: 2.2, 2.4, 2.5_

- [ ] 4. Implement authentication middleware
  - Create authMiddleware that extracts JWT from Authorization header
  - Verify token and attach decoded user data to request object
  - Return 401 status for missing, invalid, or expired tokens
  - _Requirements: 3.3, 3.4, 4.4_

- [ ] 5. Implement signup endpoint
  - [ ] 5.1 Create POST /auth/signup route and controller
    - Validate request body (name, email, password, role)
    - Check if email already exists in database
    - Hash password before storing
    - Create user record with provided role
    - Return user data (excluding password) with 201 status
    - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5, 4.1, 4.5_
  
  - [ ] 5.2 Write unit tests for signup endpoint
    - Test successful signup with USER role
    - Test successful signup with ADMIN role
    - Test duplicate email rejection
    - Test invalid role rejection
    - Test missing required fields
    - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5_

- [ ] 6. Implement login endpoint
  - [ ] 6.1 Create POST /auth/login route and controller
    - Validate request body (email, password)
    - Find user by email
    - Compare provided password with stored hash
    - Generate JWT token on successful authentication
    - Return token and user data (excluding password)
    - Return generic error message for invalid credentials
    - _Requirements: 2.1, 2.2, 2.3, 2.4, 4.2, 4.5_
  
  - [ ] 6.2 Write unit tests for login endpoint
    - Test successful login with valid credentials
    - Test failed login with invalid email
    - Test failed login with invalid password
    - Test JWT token generation
    - _Requirements: 2.1, 2.2, 2.3_

- [ ] 7. Implement user info endpoint
  - [ ] 7.1 Create GET /auth/me route and controller
    - Apply authMiddleware to protect the route
    - Extract user ID from authenticated request
    - Fetch user data from database
    - Return user information (excluding password)
    - _Requirements: 3.5, 4.3, 4.4_
  
  - [ ] 7.2 Write unit tests for /auth/me endpoint
    - Test successful retrieval with valid token
    - Test 401 response without token
    - Test 401 response with invalid token
    - _Requirements: 3.5, 4.3, 4.4_

- [ ] 8. Configure backend server and CORS
  - Set up Express server with JSON body parsing
  - Configure CORS to allow frontend domain
  - Mount authentication routes at /auth
  - Add global error handling middleware
  - Configure port from environment variable
  - _Requirements: 4.1, 4.2, 4.3_

- [ ] 9. Set up frontend project structure
  - Configure Next.js with App Router
  - Set up TailwindCSS configuration
  - Create TypeScript types for User, AuthResponse, and API requests
  - Create API client utility with Axios and base URL configuration
  - _Requirements: 6.1, 6.2, 6.3_

- [ ] 10. Implement frontend authentication context
  - Create AuthContext with user state, token state, and authentication methods
  - Implement login function that calls POST /auth/login and stores token
  - Implement logout function that clears token and user state
  - Add token persistence using localStorage
  - Add automatic token validation on app initialization
  - _Requirements: 2.2, 3.1, 3.2_

- [ ] 11. Create signup page
  - [ ] 11.1 Build signup form component
    - Create form with name, email, password, and role dropdown fields
    - Implement Zod validation schema for client-side validation
    - Use React Hook Form for form state management
    - Style form with TailwindCSS
    - _Requirements: 6.1, 6.4_
  
  - [ ] 11.2 Implement signup submission logic
    - Call POST /auth/signup endpoint on form submit
    - Display validation errors from backend
    - Redirect to /login on successful signup
    - Show success message
    - _Requirements: 1.1, 1.2, 6.4_

- [ ] 12. Create login page
  - [ ] 12.1 Build login form component
    - Create form with email and password fields
    - Implement Zod validation schema
    - Use React Hook Form for form state management
    - Style form with TailwindCSS
    - _Requirements: 6.2, 6.5_
  
  - [ ] 12.2 Implement login submission logic
    - Call login function from AuthContext
    - Display authentication errors
    - Redirect to /dashboard on successful login
    - Store JWT token in localStorage
    - _Requirements: 2.1, 2.2, 6.5_

- [ ] 13. Create protected dashboard page
  - [ ] 13.1 Build ProtectedRoute component
    - Check authentication status from AuthContext
    - Redirect to /login if not authenticated
    - Show loading state while checking authentication
    - _Requirements: 3.2, 3.4_
  
  - [ ] 13.2 Implement dashboard page
    - Wrap dashboard with ProtectedRoute component
    - Fetch current user data from GET /auth/me on mount
    - Display header with format "Welcome, [Name] (User)" or "Welcome, [Name] (Admin)"
    - Handle loading and error states
    - _Requirements: 3.1, 3.2, 3.3, 3.5, 6.3_

- [ ] 14. Configure API client with authentication
  - Set up Axios interceptor to attach JWT token to all requests
  - Add response interceptor to handle 401 errors
  - Redirect to login page on authentication failure
  - Implement centralized error handling
  - _Requirements: 3.3, 3.4_

- [ ] 15. Create comprehensive README documentation
  - Write project overview and feature list
  - Document technology stack
  - Add local setup instructions for backend (install dependencies, configure .env, run migrations)
  - Add local setup instructions for frontend (install dependencies, configure .env)
  - Document how to run both applications locally
  - Include API endpoint documentation
  - Add deployment instructions
  - _Requirements: 8.1, 8.2, 8.3, 8.4, 8.5_

- [ ] 16. Deploy backend to Render or Railway
  - Create account on deployment platform
  - Connect GitHub repository
  - Configure environment variables (DATABASE_URL, JWT_SECRET, PORT, FRONTEND_URL)
  - Set build command to install dependencies and generate Prisma client
  - Set start command to run production server
  - Run database migrations
  - Verify deployment and test endpoints
  - _Requirements: 7.2, 7.3, 7.4_

- [ ] 17. Deploy frontend to Vercel
  - Create account on Vercel
  - Connect GitHub repository
  - Configure environment variable (NEXT_PUBLIC_API_URL with backend URL)
  - Set framework preset to Next.js
  - Deploy and verify build
  - Test authentication flow on deployed application
  - _Requirements: 7.1, 7.3, 7.4_

- [ ] 18. Update README with deployment URLs
  - Add deployed frontend URL to README
  - Add deployed backend URL to README
  - Verify all links are publicly accessible
  - Test complete authentication flow on production
  - _Requirements: 7.5, 8.1_
