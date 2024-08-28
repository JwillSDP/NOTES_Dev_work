

**Project Setup & Basics:**

- **Package Manager:** Use `pnpm` for faster and more efficient package management.
- **Creating a Project:** Use `create-next-app` with the `--example` flag to start with a pre-built example.
- **Folder Structure:** Familiarize yourself with the `app`, `public`, `lib`, and `ui` directories and their purpose.
- **Placeholder Data:** Use provided placeholder data (`app/lib/placeholder-data.ts`) or external services like mockAPI for initial development.
- **TypeScript:** The project uses TypeScript for type safety and improved code quality.
- **Development Server:** Run `pnpm dev` to start the development server on port 3000.

**Styling:**

- **Global Styles:** Import `global.css` into your root layout (`app/layout.tsx`) to apply styles globally.
- **Tailwind CSS:**  The project is pre-configured with Tailwind CSS for utility-first styling.
- **CSS Modules:** An alternative styling approach that provides local scope and prevents style collisions.
- **`clsx` Library:** Use `clsx` to conditionally apply class names based on state or other conditions.

**Fonts and Images:**

- **Font Optimization (`next/font`):** Use `next/font` to optimize font loading and prevent layout shift.
- **Image Optimization (`next/image`):** Use `next/image` to automatically optimize images, prevent layout shift, resize for responsiveness, and lazy load images.

**Routing:**

- **File-System Routing:** Next.js uses file-system routing, where folders represent URL segments.
- **`page.tsx`:**  The special file that exports a React component for each route.
- **`layout.tsx`:**  Creates shared UI (like navigation) across multiple routes.
- **Colocation:**  Place UI components, tests, and other related code within the same directory as your routes.
- **Root Layout (`app/layout.tsx`):**  The top-level layout for the entire application.
- **Partial Rendering:** Only the components that change are re-rendered on navigation, improving performance.

**Linking and Navigation:**

- **`<Link>` Component:** The primary way to navigate between routes, providing prefetching and client-side transitions.
- **`useRouter` Hook:**  Use for programmatic navigation within Client Components. 
- **`redirect` Function:**  Use for redirecting on the server-side (in Server Components).
- **Native History API:** Use `pushState` and `replaceState` for more control over the browser history. 

**Deployment and Database:**

- **GitHub:** Push your code to a GitHub repository.
- **Vercel:** Connect your GitHub repo to Vercel for automated deployments and previews.
- **Postgres Database:** Create a Postgres database on Vercel and connect your app to it. 
- **Seeding:** Seed your database with initial data using a Route Handler (`app/seed/route.ts`).

**Data Fetching:**

- **Server Components:**  Fetch data directly in Server Components using the Firebase Admin SDK.
- **SQL Queries:** Use SQL to query your database efficiently.
- **Parallel Data Fetching:**  Use `Promise.all` to fetch multiple data sources in parallel.
- **Streaming:** Implement streaming with `loading.tsx` or `<Suspense>` to prevent slow data requests from blocking the entire page.
- **Route Groups:** Use parentheses in folder names (e.g., `(overview)`) to organize routes without affecting the URL structure.

**Authentication:**

- **NextAuth.js:**  Use NextAuth.js to handle authentication (e.g., with Google).
- **Middleware:** Protect routes and redirect unauthenticated users using Next.js Middleware.
- **Password Hashing:**  Use `bcrypt` to hash user passwords for security. 

**Metadata:**

- **Metadata API:**  Use Next.js's Metadata API to manage metadata (title, description, Open Graph, favicon, etc.). 
- **Special Files:**  Utilize special files in the `app` directory for static metadata (e.g., `favicon.ico`, `opengraph-image.jpg`).

**Error Handling:**

- **`error.tsx`:**  Create error boundaries for route segments to catch unexpected errors and display a fallback UI.
- **`notFound` Function:**  Use to handle 404 errors when a resource is not found. 

**Forms and Server Actions:**

- **Server Actions:**  Use Server Actions to mutate data on the server without creating API routes.
- **`useActionState`:**  Use this hook in Client Components to handle form state, submission, and errors related to Server Actions.
- **`revalidatePath`:** Revalidate the Next.js cache after a mutation.
- **`redirect`:**  Redirect the user to a different page after a mutation or action.
- **Dynamic Route Segments:** Create routes with dynamic parameters (e.g., `[id]`) to handle data-driven URLs. 
- **Form Validation:**  Use Zod for schema validation and type coercion on the server-side.
- **Accessibility:**  Use semantic HTML, labels, and focus outlines for accessible forms. 
- **Debouncing:**  Use the `use-debounce` library to optimize search functionality and prevent excessive API calls. 
- **Pagination:** Implement pagination using URL search params. 

