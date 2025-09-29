
**Goal:** Show up on Google first page for brand & local keywords like _“Example Cycles Thiruvallur”_  

  
**Step 1: Set Up Your Website for SEO**  

- Ensure your site has **unique title and meta tags**:  
  
<title>Example Cycles Thiruvallur | Premium Bicycles & Accessories</title>  
``` html
<meta name="description" content="Example Cycles offers the best bicycles in Thiruvallur. Shop Top Speed & Azpire cycles along with accessories.">  
<meta name="keywords" content="cycles in Thiruvallur, cycle shop Thiruvallur, Example Cycles">  
```
  
- Add **Open Graph tags** for social sharing:  

``` html
<meta property="og:title" content="Example Cycles Thiruvallur | Premium Bikes">  
<meta property="og:description" content="High-quality bicycles and cycling accessories in Thiruvallur.">  
<meta property="og:image" content="<https://examplecycle.com/og-image.jpg>">  
<meta property="og:url" content="<https://examplecycle.com>">  
```


- Add **Local Business Schema JSON-LD**:  

``` json
{  
  "@context": "<https://schema.org>",  
  "@type": "BicycleStore",  
  "name": "Example Cycles",  
  "url": "<https://examplecycle.com>",  
  "telephone": "+91-9876543210",  
  "address": {  
    "@type": "PostalAddress",  
    "streetAddress": "123 Cycle Street",  
    "addressLocality": "Thiruvallur",  
    "addressRegion": "Tamil Nadu",  
    "postalCode": "602001",  
    "addressCountry": "IN"  
  },  
  "openingHours": "Mo-Su 09:00-21:00"  
}  
```
  
**Step 2: Verify Ownership in Google Search Console**  

- Go to Google Search Console.  
- Click **Add Property** → choose `URL prefix` (`https://examplecycle.com`).  
- Verify ownership using **HTML meta tag** or **Google Analytics**:  

```html
<meta name="google-site-verification" content="your-verification-code" />  
```

  
**Step 3: Create Sitemap & Robots.txt**  

- **sitemap.xml** in `public/` folder:  

``` xml
<?xml version="1.0" encoding="UTF-8"?>  
<urlset xmlns="<http://www.sitemaps.org/schemas/sitemap/0.9>">  
   <url>  
      <loc><https://examplecycle.com/></loc>  
      <lastmod>2025-09-29</lastmod>  
      <priority>1.0</priority>  
   </url>  
</urlset>  
```


- **robots.txt** in `public/` folder:  

``` txt
User-agent: *  
Allow: /  
Sitemap: <https://examplecycle.com/sitemap.xml>  
```
  
- Deploy your site so both files are live:  
- `https://examplecycle.com/sitemap.xml`  
- `https://examplecycle.com/robots.txt`  

**Step 4: Submit Sitemap & Request Indexing**  

- Go to **Search Console → Sitemaps → Add new sitemap**  
- Enter: `https://examplecycle.com/sitemap.xml` → Submit  
- Use **URL Inspection Tool**:  
- Paste homepage: `https://examplecycle.com/`  
- Click **Request Indexing**  

**Step 5: Promote & Build Backlinks**  

- Share website on:  
- Instagram, Facebook, LinkedIn  
- WhatsApp groups  
- Local forums / blogs  
- Backlinks help Google discover your site faster