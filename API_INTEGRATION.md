<!-- API_INTEGRATION.md -->

# API Integration Documentation

## Overview
RentNest Frontend consumes the backend API at `http://localhost:5000/api` with role-based access control and Stripe payment integration.

## Frontend Components to Backend Endpoints

### Authentication (Auth)
| Component | Endpoint | Method | Purpose |
|-----------|----------|--------|---------|
| Register Form | `POST /api/auth/register` | POST | Create new user account |
| Login Form | `POST /api/auth/login` | POST | Authenticate user |
| Auth Store | `GET /api/auth/me` | GET | Fetch current user profile |
| Profile Update | `PATCH /api/auth/profile` | PATCH | Update user information |

### Properties (Public & Landlord)
| Component | Endpoint | Method | Purpose |
|-----------|----------|--------|---------|
| Properties Grid | `GET /api/properties` | GET | Browse all rental properties |
| Property Details | `GET /api/properties/:id` | GET | View single property details |
| Landlord Dashboard | `GET /api/landlord/properties` | GET | List landlord's properties |
| Create Property | `POST /api/landlord/properties` | POST | Add new property listing |
| Update Property | `PATCH /api/landlord/properties/:id` | PATCH | Edit property details |
| Delete Property | `DELETE /api/landlord/properties/:id` | DELETE | Remove property listing |

### Rental Requests
| Component | Endpoint | Method | Purpose |
|-----------|----------|--------|---------|
| Request Submission | `POST /api/rentals` | POST | Tenant submits rental request |
| My Requests (Tenant) | `GET /api/rentals/my-requests` | GET | View tenant's rental history |
| Request Details | `GET /api/rentals/:id` | GET | View single request details |
| Request Management | `GET /api/landlord/requests` | GET | Landlord views incoming requests |
| Approve Request | `PATCH /api/landlord/requests/:id` | PATCH | Landlord approves request |
| Reject Request | `PATCH /api/landlord/requests/:id` | PATCH | Landlord rejects request |
| Update Status | `PATCH /api/rentals/:id` | PATCH | Update request status |

### Payments & Stripe
| Component | Endpoint | Method | Purpose |
|-----------|----------|--------|---------|
| Payment Initiation | `POST /api/payments/create-session` | POST | Create Stripe checkout session |
| Payment History | `GET /api/payments/history` | GET | View tenant's payment history |
| Payment Details | `GET /api/payments/:id` | GET | Fetch payment information |
| Confirm Payment | `POST /api/payments/confirm` | POST | Confirm successful payment |
| Get by Rental | `GET /api/payments/rental/:rentalRequestId` | GET | Fetch payment for request |

### Admin Moderation
| Component | Endpoint | Method | Purpose |
|-----------|----------|--------|---------|
| User Management | `GET /api/admin/users` | GET | List all users with pagination |
| Ban User | `PATCH /api/admin/users/:id/ban` | PATCH | Ban user account |
| Unban User | `PATCH /api/admin/users/:id/unban` | PATCH | Restore banned user |
| Platform Stats | `GET /api/admin/stats` | GET | Fetch dashboard statistics |
| All Properties | `GET /api/admin/properties` | GET | View all properties for moderation |
| All Requests | `GET /api/admin/requests` | GET | View all rental requests |
| Delete Property | `DELETE /api/admin/properties/:propertyId` | DELETE | Remove inappropriate property |
| Delete Request | `DELETE /api/admin/requests/:requestId` | DELETE | Remove inappropriate request |

## Authentication Flow
All protected endpoints require JWT token in Authorization header:
```
Authorization: Bearer <JWT_TOKEN>
```

Token is stored in localStorage and automatically attached by axios interceptor.

## Error Handling
All API errors trigger user-friendly toast notifications:
- 401 Unauthorized: Auto-logout with redirect to login
- 400 Bad Request: Display validation error messages
- 500 Server Error: Generic error message

## Stripe Payment Integration
1. Tenant initiates payment via POST `/api/payments/create-session`
2. Backend returns Stripe checkout session ID
3. Frontend redirects to Stripe Checkout
4. After payment, redirects to `/payment/success` or `/payment/cancel`
5. Frontend confirms payment via POST `/api/payments/confirm`

## Environment Variables Required
```
NEXT_PUBLIC_API_URL=http://localhost:5000/api
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_xxx
NEXTAUTH_SECRET=your-secret-key
NEXTAUTH_URL=http://localhost:3000
```
