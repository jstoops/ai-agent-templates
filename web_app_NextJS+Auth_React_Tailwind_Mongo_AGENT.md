## Business Requirements

This project is building a [project name] App for listing and finding rental properties. Key features:
- Anyone can sign in with Google; the first sign-in creates a user profile with their name, email, and avatar
- Visitors can browse paginated and featured listings, search by location and property type, and view a listing's details, image gallery, and location on a map
- A signed-in user can create, edit, and delete their own listings, including uploading multiple images
- A signed-in user can bookmark listings and see them on a saved-properties page; their profile shows the listings they own
- A signed-in user can send a message to a listing's owner; the owner can read messages, mark them read or unread, delete them, and see an unread count in the navbar
- Listings can be shared to social media; the UI gives toast feedback, loading spinners, and custom error and 404 pages

## Limitations

Google OAuth is the only way to sign in; there is no email/password registration and no way to delete an account in the app.

Listings can only be managed by the user who created them; there are no admin or shared-management roles.

Search is a case-insensitive text match on the listing's name, description, and address fields, plus an optional property type filter.

MongoDB, Google OAuth, Cloudinary, Mapbox, and Google Geocoding credentials must be supplied in `.env`. The app runs locally with `npm run dev`; there is no Docker setup.

## Technical Decisions

- Next.js (App Router) with React, written in JavaScript (`.js`/`.jsx`), with the `@/` import alias
- No separate backend: pages in `app/` are async server components that query the database directly; mutations are Next.js server actions in `app/actions/` that call `revalidatePath` afterwards; route handlers in `app/api/` are kept to a minimum
- Mark components `'use client'` only when they need browser state or interactivity
- Tailwind CSS for responsive styling
- NextAuth.js with the Google provider; the `signIn` callback creates the MongoDB user and the `session` callback adds the user's database ID to the session. Server code gets the current user with `getSessionUser()` in `utils/`
- `middleware.js` protects `/properties/add`, `/profile`, `/properties/saved`, and `/messages`; every server action also checks the session and verifies ownership before changing a listing or message
- MongoDB with Mongoose for `User`, `Property`, and `Message` models in `models/`; connect with the cached `connectDB()` in `config/database.js` using `MONGO_URI`. Convert `.lean()` documents with `convertToSerializeableObject` before passing them to client components
- Cloudinary for image uploads (folder `propertypulse`); store the image URLs on the property and delete the images from Cloudinary when a listing is deleted
- Mapbox GL with React Map GL for maps, with React Geocode (Google Geocoding API) to turn addresses into coordinates
- React Context (`context/GlobalContext.js`) for the unread message count
- React PhotoSwipe Gallery for the image lightbox, React Toastify for notifications, React Spinners for loading, React Share for social sharing, React Icons for icons
- Jest and React Testing Library in `__tests__/`, with `npm test` and `npm run test:coverage`; the coverage threshold is 100%

## Coding Standards

- Follow the coding standards and coding style in the [jstoops/next-property](https://github.com/jstoops/next-property) repo, and use its code as the example to match for structure, naming, and patterns
