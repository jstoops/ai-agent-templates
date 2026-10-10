## Business Requirements

This project is building a [project name] e-commerce store App. Key features:
- Anyone can register with name, email, and password and sign in; a signed-in user can update their name, email, and password on their profile page
- Visitors can browse paginated products, search by keyword, see a carousel of the top-rated products, and view product details with ratings and reviews
- Anyone can add in-stock products to a cart and change quantities; the cart is kept in the browser
- A signed-in user checks out in steps: shipping address, payment method, then place order; orders are paid with PayPal or a card through PayPal
- A signed-in user can see their orders on their profile page and leave one review per product
- Admins can manage products (create, edit with image upload, delete), users (edit name, email, and admin status; delete non-admin users), and orders (view all, mark delivered)

## Limitations

Seeded demo accounts are `admin@email.com` / `123456` (admin), `john@email.com` / `123456`, and `jane@email.com` / `123456`; `npm run data:import` resets the database with sample products and users and `npm run data:destroy` clears it.

Sign-in is email and password only; there are no OAuth providers, email verification, password reset, or self-service account deletion.

Users are either regular users or admins (`isAdmin`). Product search matches on name only.

Prices use fixed rules: free shipping over $100 (otherwise $10) and 15% tax. PayPal is the only payment provider.

Uploaded images are saved to the local `uploads/` folder (`/var/data/uploads` in production), not to cloud storage.

MongoDB, `JWT_SECRET`, `PAGINATION_LIMIT`, and PayPal credentials must be supplied in `.env` (see `.env.example`). The app runs locally with `npm run dev` and is deployed to Render; there is no Docker setup.

## Technical Decisions

- MERN stack in JavaScript with ES modules: an Express API in `backend/` and a React single-page app in `frontend/` (Create React App), each with its own `package.json`
- Express backend organized as `routes/` → `controllers/` → `models/`, with all endpoints under `/api` (`/api/products`, `/api/users`, `/api/orders`, `/api/upload`, `/api/config/paypal`); `app.js` builds the app and `server.js` connects to the database and starts it
- Wrap controllers in `asyncHandler`; set `res.status()` and `throw new Error()` for failures, and let `notFound` and `errorHandler` in `errorMiddleware.js` send JSON errors. Use `checkObjectId` on routes that take an `:id`
- In production Express serves the built React app from `frontend/build` and the uploaded images; in development the React dev server proxies API calls to `http://localhost:5000`, and `concurrently` with `nodemon` runs both
- MongoDB with Mongoose for `User`, `Product` (with embedded reviews), and `Order` (with embedded order items, shipping address, and payment result) models; connect with `connectDB()` in `config/db.js` using `MONGO_URI`
- Hash passwords with `bcryptjs` in a Mongoose `pre('save')` hook and check them with `user.matchPassword()`
- Authenticate with a JWT (`jsonwebtoken`, 30-day expiry) stored in an HTTP-only, `sameSite: 'strict'` cookie named `jwt` and read with `cookie-parser`; the `protect` middleware loads `req.user` and the `admin` middleware requires `isAdmin`. Every route that changes data must use `protect`, admin routes must also use `admin`, and order and upload routes must check that the user owns the data
- Compute order prices on the server with `calcPrices()` from product prices in the database, never from prices sent by the client
- Verify PayPal payments on the server (`utils/paypal.js`) and reject reused transaction IDs before marking an order paid; the frontend uses React PayPal JS with the client ID from `/api/config/paypal`
- Multer for image uploads to `uploads/`, allowing only JPG, PNG, and WebP files
- Redux Toolkit for frontend state: RTK Query in `apiSlice` with endpoints injected from `productsApiSlice`, `usersApiSlice`, and `ordersApiSlice`, plus `authSlice` and `cartSlice` saved to `localStorage`. The base query logs the user out on any 401
- React Router for routing, with `PrivateRoute` and `AdminRoute` wrappers; frontend pages are `screens/` and shared pieces are `components/`
- React Bootstrap (with a custom Bootstrap theme) for UI, React Icons for icons, React Toastify for notifications, and React Helmet Async for page titles and meta tags
- Backend tests use Node's built-in test runner with Supertest and `mongodb-memory-server` in `backend/tests/`; frontend tests use Jest and React Testing Library. `npm test` runs both, and `npm run test:backend` and `npm run test:frontend` run each one

## Coding Standards

- Follow the coding standards and coding style in the [jstoops/proshop](https://github.com/jstoops/proshop) repo, and use its code as the example to match for structure, naming, and patterns
