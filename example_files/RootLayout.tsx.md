
### 📁 `src/layouts/RootLayout.tsx`

```tsx
import React, { ReactNode } from "react";
import { Helmet } from "react-helmet-async";
import Navbar from "./Navbar";
import "../globals.css";

interface RootLayoutProps {
  children: ReactNode;
}

export default function RootLayout({ children }: RootLayoutProps) {
  return (
    <html lang="en">
      <Helmet>
        {/* ✅ Base Meta */}
        <title>
          Lolito Cycle Thiruvallur | Top Speed & Azpire Bicycles, Gear &
          Accessories
        </title>
        <meta
          name="description"
          content="Looking for cycles in Thiruvallur district? Shop premium bicycles from Lolito Cycle including Top Speed and Azpire series. Best cycle shop in Thiruvallur with gear & accessories."
        />
        <meta
          name="keywords"
          content="cycles in Thiruvallur district, cycle shop Thiruvallur, Lolito Cycle Thiruvallur, Top Speed Cycle Thiruvallur, Azpire Cycle Thiruvallur, buy bicycles Thiruvallur, cycling accessories Thiruvallur, mountain bikes Thiruvallur, road bikes Thiruvallur, bike gear Thiruvallur"
        />
        <meta name="robots" content="index, follow" />
        <link rel="canonical" href="https://www.lolitocycle.in" />

        {/* ✅ Favicons */}
        <link rel="icon" href="/favicon.ico" sizes="any" />
        <link rel="apple-touch-icon" href="/apple-touch-icon.png" />

        {/* ✅ OpenGraph */}
        <meta property="og:title" content="Lolito Cycle Thiruvallur | Premium Bicycles & Accessories" />
        <meta
          property="og:description"
          content="Explore Lolito Cycle in Thiruvallur district. High-performance bicycles, Top Speed and Azpire series, gear & accessories for every rider."
        />
        <meta property="og:url" content="https://www.lolitocycle.com" />
        <meta property="og:site_name" content="Lolito Cycle" />
        <meta property="og:type" content="website" />
        <meta
          property="og:image"
          content="https://www.lolitocycle.com/og-image.jpg"
        />

        {/* ✅ Twitter */}
        <meta name="twitter:card" content="summary_large_image" />
        <meta
          name="twitter:title"
          content="Lolito Cycle Thiruvallur | Buy Top Speed & Azpire Bicycles"
        />
        <meta
          name="twitter:description"
          content="Buy Top Speed and Azpire bicycles from Lolito Cycle in Thiruvallur district. Quality bikes built for performance and style."
        />
        <meta
          name="twitter:image"
          content="https://www.lolitocycle.com/og-image.jpg"
        />
        <meta name="twitter:creator" content="@LolitoCycle" />

        {/* ✅ Google Verification */}
        <meta
          name="google-site-verification"
          content="VZgv8zZsVf-9ddDddhZiagDaUCHVCOBHetR9wbRHzew"
        />

        {/* ✅ JSON-LD Schema */}
        <script type="application/ld+json">
          {JSON.stringify({
            "@context": "https://schema.org",
            "@graph": [
              {
                "@type": "Organization",
                name: "Lolito Cycle",
                image: "https://www.lolitocycle.com/og-image.jpg",
                logo: "https://lolitocycle.in/logo.png",
                "@id": "https://www.lolitocycle.com",
                url: "https://www.lolitocycle.com",
                telephone: "+91-8778066961",
                address: {
                  "@type": "PostalAddress",
                  streetAddress: "123 Cycle Street",
                  addressLocality: "Thiruvallur",
                  addressRegion: "Tamil Nadu",
                  postalCode: "602001",
                  addressCountry: "IN",
                },
                openingHours: "Mo-Su 09:00-21:00",
                sameAs: [
                  "https://www.facebook.com/lolitocycle",
                  "https://www.instagram.com/lolitocycle",
                ],
              },
              {
                "@type": "Product",
                name: "Top Speed 26 Flash",
                image: [
                  "https://lolitocycle.in/top-speed-26-flash-ms.png",
                  "https://lolitocycle.in/top-speed-26-flash-ss-1.png",
                  "https://lolitocycle.in/top-speed-26-flash-ss-2.png",
                ],
                description:
                  "Top Speed 26 Flash Bicycle available at Lolito Cycles, Thiruvallur",
                brand: "Top Speed",
                offers: {
                  "@type": "Offer",
                  url: "https://lolitocycle.in",
                  priceCurrency: "INR",
                  availability: "https://schema.org/InStock",
                },
              },
              {
                "@type": "Product",
                name: "Top Speed 26 Tornado",
                image: [
                  "https://lolitocycle.in/top-speed-26-tornado-ss-1.png",
                  "https://lolitocycle.in/top-speed-26-tornado-ss-2.png",
                ],
                description:
                  "Top Speed 26 Tornado Bicycle available at Lolito Cycles, Thiruvallur",
                brand: "Top Speed",
                offers: {
                  "@type": "Offer",
                  url: "https://lolitocycle.in",
                  priceCurrency: "INR",
                  availability: "https://schema.org/InStock",
                },
              },
            ],
          })}
        </script>
      </Helmet>

      <body className="antialiased">
        <Navbar />
        {children}
      </body>
    </html>
  );
}
```

---

### 📁 `src/main.tsx`

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { HelmetProvider } from "react-helmet-async";
import RootLayout from "./layouts/RootLayout";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <HelmetProvider>
      <RootLayout>
        <div className="p-6">
          <h1 className="text-3xl font-bold">Welcome to Lolito Cycle 🚴</h1>
        </div>
      </RootLayout>
    </HelmetProvider>
  </React.StrictMode>
);
```

---

✅ Now your `RootLayout` is **completely reusable in Vite**:

- You can drop in any `<Navbar />` component.
- Change `<Helmet>` tags per page or extend with page-level SEO.
- JSON-LD is injected properly.

---
### 🔹 What is Helmet?

**Helmet** (or more often **React Helmet Async**) is a React library that lets you **manage `<head>` tags** dynamically in your app.

It allows you to set things like:

- `<title>`
- `<meta name="description">`
- `<meta property="og:...">` (OpenGraph)
- `<meta name="twitter:...">`
- `<link rel="canonical">`
- `<script type="application/ld+json">` (for SEO structured data)

---

### 🔹 Why do we use it?

In plain Vite + React:

- The `index.html` file has a single `<head>` section → not dynamic.
- Without Helmet, every page would share the same title + metadata.

With Helmet:

- Each page (or layout) can **inject its own metadata** into the `<head>` when that page is rendered.
- This is super important for **SEO** and **social sharing previews**.

---
### 🔹 Example

```tsx
import { Helmet } from "react-helmet-async";

export default function AboutPage() {
  return (
    <>
      <Helmet>
        <title>About Us | Lolito Cycle</title>
        <meta
          name="description"
          content="Learn more about Lolito Cycle, the best bicycle shop in Thiruvallur."
        />
        <link rel="canonical" href="https://www.lolitocycle.in/about" />
      </Helmet>

      <h1>About Lolito Cycle 🚴</h1>
      <p>We sell Top Speed & Azpire bicycles in Thiruvallur.</p>
    </>
  );
}
```

👉 When the `AboutPage` renders, the `<head>` of your site updates to reflect those tags.

---

⚡ In Next.js, this is handled automatically by the `metadata` API.  
⚡ In Vite (React), we need `react-helmet-async` to replicate that behavior.
