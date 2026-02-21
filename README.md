# Bundle Banner Section – Development Approach & A/B Testing

## 1. Section development approach

### 1.1 Overview

The **Bundle banner** is a standalone Shopify section that promotes curated bundles with a headline, description, CTA, and product imagery.

**Files:**

| File | Purpose |
|------|--------|
| `sections/bundle-banner.liquid` | Section markup, schema, and block resolution |
| `assets/bundle-banner.css` | All section styles (BEM, mobile-first) |

**Principles:**

- **Mobile-first CSS** – Base styles for small screens; `750px` breakpoints for larger layouts.
- **BEM naming** – All section classes follow `.bundle-banner`, `.bundle-banner__element`, `.bundle-banner__element--modifier`.

---

### 1.2 Schema design

**Section settings (global for the section):**

- **Background** – `background_image` (desktop), `background_image_mobile` (mobile).
- **Colors** – `text_color`, `highlight_color` (used for heading highlight and CTA button).

**Blocks (limit 1 per type):**

- **Heading** – `heading`, `heading_highlight` (optional accent phrase).
- **Description** – `description` (textarea).
- **Button (CTA)** – `cta_label`, `cta_link` (collection or any URL).
- **Product image** – `product_image` (desktop), `product_image_mobile` (mobile), `product_image_alt`.

---

### 1.3 Liquid structure

1. **First-section detection** – `section.index == 1` is used to set `fetchpriority="high"` and `loading="eager"` for above-the-fold images when the section is the first on the page.
3. **CSS variables** – Section settings for colors are output as inline style on the section: `--bundle-banner-text-color`, `--bundle-banner-highlight-color`.
4. **Picture for art direction** – Background and product image each use a `<picture>` element with:
   - `<source media="(max-width: 749px)" srcset="...">` for mobile when both mobile and desktop images exist.
   - `<img>` as fallback for desktop (or only image when one asset is set).  
---

### 1.4 Transparent header (desktop)

When the header is transparent and the Bundle banner is the **first section** on the page:

- **Desktop only (750px+):** `.bundle-banner__content` gets  
  `padding-top: calc(var(--header-group-height, 0px) + 60px);`  
---


## 2. A/B testing: Hero vs Bundle banner on PDP

### 2.1 Test requirements (mandatory)

Created a test in **Intelligems** :

### 2.2 Prerequisites

- Intelligems  app installed and configured on the **dev store**.

### 2.3 Option A: Template (section order) test (recommended)

Use **two product templates** that differ only by which section is first: one “control” (current hero section) and one “variant” (bundle banner first).

**Steps:**

1. **Duplicate the product template**  
   In Theme editor: **Online Store → Themes → Customize → Product page**.  
   Use “Add template” or duplicate the current product template and name them, e.g.:
   - `product` (control – current layout,  hero section).
   - `product.bundle-banner` (variant – Bundle banner instead of hero section).


2. **Create the test in Intelligems**
   - Content Testing → New test.
   - **Type:** “Template test”.
   - **Page scope:** Product pages (PDP).
   - **Control:** Template `product` (current experience).
   - **Variant(s):** Template `product.bundle-banner` (Bundle banner ).
   - Set the  **traffic split 50/50**, **primary metric: add-to-cart rate**, **secondary metric: revenue per session**, and **target: product pages only**. Run the test.

3. **How it works**  
   Intelligems will assign visitors to control or variant and serve the corresponding template on product pages so that one group sees the hero  section and the other sees the Bundle banner.

---

### 2.8 Test configuration 


| Field | Value |
|-------|--------|
| **Platform** | Intelligems pm |
| **Store** | https://pm-digital-test-store.myshopify.com/ |
| **Test name** | PDP – Hero vs Bundle banner  |
| **Test ID** | b3d0ddb5-154f-4223-a33d-8d3be66aef0e |
| **Test type** | Template test |
| **Control** | Existing hero section on PDP |
| **Variation** | Bundle banner section on PDP |
| **Traffic split** | 50% control / 50% variation |

#### URL targeting rules

| Rule | Setting | Notes |
|------|---------|--------|
| **Target** | Product pages only | Test must **not** run on homepage or collection pages. |
| **Include URLs** | Product pages only | |
| **URL pattern** | *e.g. `*/products/*` or `.*/products/.*`* | Match all PDPs. Document the exact pattern used in your platform. |
| **Exclude URLs** | Homepage, collection pages | **Do not** run test on: `/` (homepage), `/collections/*`, or any non-product pages. |
| **Exclude pattern** | *e.g. `^/$`, `*/collections/*`* | Document the exact exclude rules you configured. |

**Example:**

- **Include:** Template = Product, **or** URL contains `/products/`.
- **Exclude:** URL equals `/`, URL contains `/collections/`.

#### Metrics setup

| Metric type | Metric name | Goal | Notes |
|-------------|-------------|------|--------|
| **Primary** | Add-to-cart rate | Increase | Main decision metric; use for statistical significance and winner. |
| **Secondary** | Revenue per session | Increase | |
