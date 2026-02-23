# ShopVerse - E-Commerce Platform

## Overview
A full-featured Shopify-like e-commerce platform with product browsing, phone+OTP authentication, cart/wishlist management, and UPI payment checkout.

## Tech Stack
- **Frontend**: React 18 + Vite, Tailwind CSS, shadcn/ui, wouter (routing), TanStack Query
- **Backend**: Express.js, express-session for auth
- **Database**: PostgreSQL with Drizzle ORM
- **Auth**: Phone number + OTP verification (demo mode - OTP shown in response)

## Project Structure
```
client/src/
  App.tsx            - Main app with routing
  lib/auth.tsx       - Auth context & provider
  lib/theme.tsx      - Dark/light theme provider
  components/
    layout/header.tsx  - Navigation header with cart/wishlist badges
    layout/footer.tsx  - Site footer
    product-card.tsx   - Reusable product card component
  pages/
    home.tsx          - Landing page with hero, categories, featured products
    products.tsx      - Product listing with filters, categories, search, sort
    product-detail.tsx - Single product view with add-to-cart
    cart.tsx           - Shopping cart with quantity management
    wishlist.tsx       - Wishlist with move-to-cart
    login.tsx          - Phone+OTP login flow (3 steps: phone, otp, profile)
    profile.tsx        - User profile, addresses, order history
    checkout.tsx       - Multi-step checkout (address, UPI payment, review)
    orders.tsx         - Order history

server/
  index.ts           - Express server entry point
  db.ts              - Database connection (pg + drizzle)
  storage.ts         - DatabaseStorage class implementing IStorage interface
  routes.ts          - All API routes with session-based auth
  seed.ts            - Database seeding with categories & products

shared/
  schema.ts          - Drizzle schema definitions + Zod validation schemas
```

## Database Schema
- **users**: id, phone, name, email, verified
- **otp_codes**: id, phone, code, expires_at
- **categories**: id, name, slug, description, image
- **products**: id, name, slug, description, price, compare_price, images[], category_id, stock, rating, reviews_count, featured, badge
- **cart_items**: id, user_id, product_id, quantity
- **wishlist_items**: id, user_id, product_id
- **addresses**: id, user_id, name, phone, address_line1, address_line2, city, state, pincode, is_default
- **orders**: id, user_id, status, total, address_id, payment_method, payment_status, upi_id, created_at
- **order_items**: id, order_id, product_id, quantity, price

## API Endpoints
- `POST /api/auth/send-otp` - Send OTP (demo: returns OTP in response)
- `POST /api/auth/verify-otp` - Verify OTP & login
- `PATCH /api/auth/profile` - Update profile (name, email)
- `POST /api/auth/logout` - Logout
- `GET /api/auth/me` - Get current user
- `GET /api/categories` - List categories
- `GET /api/products` - List products (with ?category, ?search, ?featured, ?sort, ?limit)
- `GET /api/products/:slug` - Get product by slug
- `GET/POST/PATCH/DELETE /api/cart` - Cart CRUD
- `GET/POST/DELETE /api/wishlist` - Wishlist CRUD
- `GET/POST/DELETE /api/addresses` - Address CRUD
- `GET/POST /api/orders` - Order management

## Key Features
1. Phone+OTP authentication (demo mode)
2. Product catalog with categories, search, sort, filters
3. Shopping cart with quantity management
4. Wishlist with move-to-cart functionality
5. Multi-step checkout with UPI payment
6. User profile with address management
7. Order history tracking
8. Dark/light mode toggle
9. Responsive design (mobile-first)

## Running Locally
See `LOCAL_SETUP.md` for a detailed step-by-step guide. Quick summary:
1. Install Node.js 20+ and PostgreSQL
2. Clone the project
3. Copy `.env.example` to `.env` and fill in your database URL and session secret
4. Run `npm install`
5. Run `npm run db:push` to create tables
6. Run `npm run dev` for development
7. Run `npm run build && npm start` for production

## Recent Changes
- Initial build: Complete e-commerce platform with all core features
- Added LOCAL_SETUP.md with detailed local deployment instructions
- Added .env.example for easy environment configuration
