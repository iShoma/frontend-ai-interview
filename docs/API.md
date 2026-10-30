# API

Public API at `https://dummyjson.com`. 194 products in total.

| What                            | Request                                                                     |
| ------------------------------- | --------------------------------------------------------------------------- |
| List                            | `GET /products?limit=20&skip=0`                                             |
| Search (title and description)  | `GET /products/search?q=phone`                                              |
| Sorting                         | `&sortBy=price&order=asc` or `&order=desc` (any field: `price`, `rating`, …) |
| Only the fields you need        | `&select=title,price,discountPercentage,rating,thumbnail`                   |
| Category list                   | `GET /products/category-list`                                               |
| Products in a category          | `GET /products/category/smartphones`                                        |
| Single product                  | `GET /products/12`                                                          |

## Data models

The types below were taken from real API responses and can be used as is.

```ts
// GET /products, /products/search, /products/category/:slug
// With select, product objects contain only the listed fields (plus id)
type ProductListResponse = {
  products: Product[];
  total: number; // total matches, regardless of limit/skip
  skip: number;
  limit: number;
};

// GET /products/:id
type Product = {
  id: number; // 1 to 194
  title: string;
  description: string;
  category: string; // slug from /products/category-list, e.g. "smartphones"
  price: number; // in dollars, e.g. 9.99
  discountPercentage: number; // e.g. 10.48
  rating: number; // 0 to 5, e.g. 2.56
  stock: number; // units left in stock, can be 0
  tags: string[];
  brand?: string; // missing on some products
  sku: string;
  weight: number;
  dimensions: { width: number; height: number; depth: number };
  warrantyInformation: string;
  shippingInformation: string;
  availabilityStatus: "In Stock" | "Low Stock" | "Out of Stock";
  reviews: Review[]; // always 3
  returnPolicy: string;
  minimumOrderQuantity: number;
  meta: {
    createdAt: string;
    updatedAt: string;
    barcode: string;
    qrCode: string;
  };
  images: string[]; // 1 to 6 URLs
  thumbnail: string; // thumbnail URL
};

type Review = {
  rating: number; // 1 to 5
  comment: string;
  date: string; // ISO 8601
  reviewerName: string;
  reviewerEmail: string;
};

// GET /products/category-list
type CategoryList = string[]; // 24 slugs: "beauty", "laptops", "smartphones", …

// Error, e.g. GET /products/999 returns 404
type ApiError = { message: string }; // "Product with id '999' not found"
```

A search with no results returns a regular response: `{ "products": [], "total": 0, "skip": 0, "limit": 0 }`.

## Debugging

- `?delay=2000` on any request delays the response by that many milliseconds.
- `GET /http/500`, `GET /http/503` return a response with the given error status.
