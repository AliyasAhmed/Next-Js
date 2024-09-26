# Automatic Installation
```terminal
npx create-next-app@latest
```

On installation, you'll see the following prompts:

```terminal
What is your project named? my-app
Would you like to use TypeScript? No / Yes
Would you like to use ESLint? No / Yes
Would you like to use Tailwind CSS? No / Yes
Would you like your code inside a `src/` directory? No / Yes
Would you like to use App Router? (recommended) No / Yes
Would you like to use Turbopack for `next dev`?  No / Yes
Would you like to customize the import alias (`@/*` by default)? No / Yes
What import alias would you like configured? @/*
```

After the prompts, `create-next-app` will create a folder with your project name and install the required dependencies.



You're absolutely right! I appreciate the correction.

In the **newer versions of Next.js** (especially after version 13), there's a shift toward the **App Router** using folders and files in a more structured way. Let's clarify this:

### Folder Structure in Next.js (App Router - Next.js 13+)

In **Next.js 13** and beyond, the file structure for routing is different. Instead of placing pages directly inside a `pages` folder, you use specific directories with **`page.js`** files inside them. For example:

- **`app/home/page.js`** will serve as the homepage (`/`).
- **`app/contact/page.js`** will serve as the contact page (`/contact`).

Now let’s correct the examples based on this.

### Example 1: Home Page

File path: **`app/home/page.js`**

```jsx
import React from 'react';

const page = () => {
  return (
    <div>
      I am home page
    </div>
  );
};

export default page;
```

This is the home page (`/home`). You need the `page.js` file inside a folder (e.g., `home`).

### Example 2: Navbar Component

```jsx
import React from 'react';

const Navbar = () => {
  return (
    <div>
      I am navbar
    </div>
  );
};

export default Navbar;
```

This `Navbar` can be reused across pages by adding it to a **layout** file. In Next.js 13, each section (like `/home` or `/contact`) can have its own layout.

### Example 3: Root Layout (Using Navbar)

File path: **`app/layout.js`**

```jsx
import Navbar from "@/component/Navbar";  // Import the Navbar

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <Navbar />  {/* Navbar is included here, visible on all pages */}
        {children}  {/* This will render the content of the page */}
      </body>
    </html>
  );
}
```

This `layout.js` file is now the main layout for the entire app. All pages (e.g., `/home`, `/contact`) will share this layout with the navbar at the top.

### Example 4: Contact Page

File path: **`app/contact/page.js`**

```jsx
import React from 'react';

const page = () => {
  return (
    <div>
      I am contact page
    </div>
  );
};

export default page;
```

This is the contact page, and it will be accessible via `/contact`.

---

### Key Takeaways for Next.js 13+ Routing:
- The **`app`** directory is now used for routing.
- Each folder inside **`app`** (like `home` or `contact`) should have a **`page.js`** file for the route.
- You can use **`layout.js`** files to define a common structure (like a Navbar) that wraps multiple pages.

So in Next.js 13 and beyond:
- `app/home/page.js` becomes `/home`.
- `app/contact/page.js` becomes `/contact`.
- The **`page.js`** file is crucial to load any route.

Does this version match your understanding and feel clearer now?
