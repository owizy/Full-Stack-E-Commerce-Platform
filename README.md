# Full-Stack E-Commerce Platform

A modern, open-source **single-store e-commerce platform** built with **Next.js, Express, Prisma, and PostgreSQL**. The application provides a complete shopping experience with authentication, product management, cart and checkout, Stripe payments, order tracking, admin analytics, inventory management, and real-time customer support.





---

## ✨ Features

### 🛍️ Storefront

* Browse products and categories
* Product detail pages
* Shopping cart
* Secure checkout
* Stripe payment integration
* Order history
* Order tracking
* Responsive shopping experience

### 🔐 Authentication & Authorization

* User registration and sign-in
* Role-based access control
* Customer, Admin, and Superadmin roles
* Protected API routes
* Session/JWT-based authentication

### 📊 Admin Dashboard

* Dashboard overview
* Product management
* Inventory management
* Product attributes
* Sales and business analytics
* Reports
* Audit logs
* Order management
* Admin customer support chat

### 💬 Real-Time Chat

* Customer support messaging
* Real-time communication with Socket.IO
* Customer chat interface
* Admin chat dashboard

### ⚙️ Backend & API

* Express REST API
* Prisma ORM
* PostgreSQL database
* Redis integration
* Stripe payments
* Cloudinary integration
* Swagger API documentation
* API available at `/api/v1`
* Swagger documentation available at `/api-docs`

### 🧑‍💻 Developer Experience

* Docker Compose development environment
* Database migrations
* Seed data
* Environment variable templates
* Demo mode
* Production deployment support
* Modular frontend and backend architecture

---

## 📸 Screenshots

### Storefront

| Home                                         | Product Details                                          |
| -------------------------------------------- | -------------------------------------------------------- |
| ![Homepage](assets/screenshots/homepage.png) | ![Product detail](assets/screenshots/product_detail.png) |

| Cart                                 | Checkout                                   |
| ------------------------------------ | ------------------------------------------ |
| ![Cart](assets/screenshots/cart.png) | ![Payment](assets/screenshots/payment.png) |

| Sign In                                    | Sign Up                                    |
| ------------------------------------------ | ------------------------------------------ |
| ![Sign in](assets/screenshots/sign-in.png) | ![Sign up](assets/screenshots/sign-up.png) |

| Your Orders                                        | Track Order                                                  |
| -------------------------------------------------- | ------------------------------------------------------------ |
| ![Your orders](assets/screenshots/your_orders.png) | ![Track your order](assets/screenshots/track_your_order.png) |

| Customer Chat                                      |
| -------------------------------------------------- |
| ![Customer chat](assets/screenshots/user_chat.png) |

### Admin Dashboard

| Overview                                                         | Products                                                         |
| ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| ![Dashboard overview](assets/screenshots/dashboard_overview.png) | ![Products dashboard](assets/screenshots/products_dashboard.png) |

| Analytics                                                | Inventory                                                |
| -------------------------------------------------------- | -------------------------------------------------------- |
| ![Analytics](assets/screenshots/analytics_dashboard.png) | ![Inventory](assets/screenshots/inventory_dashboard.png) |

| Attributes                                                 | Reports                                              |
| ---------------------------------------------------------- | ---------------------------------------------------- |
| ![Attributes](assets/screenshots/attributes_dashboard.png) | ![Reports](assets/screenshots/reports_dashboard.png) |

| Logs                                           | Admin Chat                                           |
| ---------------------------------------------- | ---------------------------------------------------- |
| ![Logs](assets/screenshots/logs_dashboard.png) | ![Admin chat](assets/screenshots/dashboard_chat.png) |

---


## 🏗️ Full-Stack Deployment

This repository does not provide a permanently hosted production API, PostgreSQL database, or Redis instance.

To run the complete application, you can either:

* Run the project locally with Docker
* Run the services directly with Node.js
* Deploy the frontend and backend yourself

For a self-hosted deployment, you will need:

* Next.js frontend
* Express API
* PostgreSQL
* Redis
* Stripe configuration
* Environment variables
* A production-ready hosting provider

### Frontend Environment

Configure:

```env
NEXT_PUBLIC_API_URL_PROD=your_api_url
NEXT_PUBLIC_API_URL_DEV=your_development_api_url
NEXT_PUBLIC_SOCKET_URL=your_socket_url
```

See:

```text
src/client/.env.example
```

### Backend Environment

Configure values such as:

```env
DATABASE_URL=your_database_url
REDIS_URL=your_redis_url
JWT_SECRET=your_jwt_secret
```

Additional Stripe, OAuth, and application settings are documented in:

```text
src/server/.env.example
```

### CORS

Set `ALLOWED_ORIGINS` on the backend to your frontend URL.

Multiple origins can be provided as a comma-separated list.

### Database

For production deployments, use a managed PostgreSQL and Redis provider.

Run Prisma migrations with:

```bash
npx prisma migrate deploy
```

---

## 📁 Project Structure

```text
src/
├── client/                 # Next.js frontend
├── server/                 # Express API + Prisma
├── docker-compose.yml       # Docker development environment
└── .env.example             # Docker PostgreSQL configuration
```

---

# 🛠️ Local Setup

## Prerequisites

Make sure you have the following installed:

* Node.js 18+
* npm
* PostgreSQL
* Redis
* Docker and Docker Compose — recommended

You can use Docker Compose to run PostgreSQL and Redis locally without installing them separately.

---

## 1. Clone the Repository

```bash
git clone https://github.com/owizy/full-stack-e-commerce-platform.git
cd full-stack-e-commerce-platform
```

---

## 2. Create Environment Files

From the project root:

```bash
cp src/.env.example src/.env
cp src/server/.env.example src/server/.env
cp src/client/.env.example src/client/.env.local
```

Then update the environment variables according to your local configuration.

### Environment Files

| File                    | Purpose                                                         |
| ----------------------- | --------------------------------------------------------------- |
| `src/.env`              | PostgreSQL credentials for Docker                               |
| `src/server/.env`       | API, database, Redis, authentication, and payment configuration |
| `src/client/.env.local` | Frontend API and Socket.IO URLs                                 |

For Docker development, make sure the PostgreSQL credentials match between `src/.env` and the server's `DATABASE_URL`.

Example:

```env
DATABASE_URL=postgresql://YOUR_USER:YOUR_PASSWORD@localhost:5432/b2c_ecommerce
```

Inside Docker Compose, the database host is overridden to use the `db` service.

---

# 🐳 Option A — Docker

Docker Compose is the recommended way to run the full development environment.

From the `src` directory:

```bash
cd src
docker compose up --build
```

Once the containers are running, open a second terminal and run:

```bash
cd src
docker compose exec server npx prisma migrate deploy
docker compose exec server npm run seed
```

### Application URLs

| Service  | URL                            |
| -------- | ------------------------------ |
| Frontend | http://localhost:3000          |
| API      | http://localhost:5000/api/v1   |
| Swagger  | http://localhost:5000/api-docs |

---

## ⚠️ PostgreSQL Volume Reset

Docker PostgreSQL reads:

```env
POSTGRES_USER
POSTGRES_PASSWORD
POSTGRES_DB
```

from `src/.env` when the database volume is created for the first time.

If you change these credentials after the volume already exists, PostgreSQL will continue using the old credentials.

This can result in an error such as:

```text
P1000: Authentication failed
```

### Reset the Local Database

If you need to recreate the database with the current credentials:

```bash
cd src
docker compose down -v
docker compose up --build
docker compose exec server npx prisma migrate deploy
docker compose exec server npm run seed
```

> **Warning:** `docker compose down -v` deletes the local PostgreSQL data stored in the Docker volume.

---

# 💻 Option B — Without Docker

You can also run the frontend and backend directly with Node.js.

### Backend

```bash
cd src/server
npm install
npx prisma migrate dev
npm run seed
npm run dev
```

### Frontend

Open another terminal:

```bash
cd src/client
npm install
npm run dev
```

The frontend will normally be available at:

```text
http://localhost:3000
```

---

# 👤 Test Accounts

After running the database seed:

```bash
npm run seed
```

you can use the following development accounts:

| Role       | Email                    | Password      |
| ---------- | ------------------------ | ------------- |
| Superadmin | `superadmin@example.com` | `password123` |
| Admin      | `admin@example.com`      | `password123` |
| User       | `user@example.com`       | `password123` |

> These accounts are intended for local development only.

---

# 🧪 Demo Mode

The application supports multiple demo and fallback modes.

| Environment Variable                | Description                                   |
| ----------------------------------- | --------------------------------------------- |
| `NEXT_PUBLIC_DEMO_MODE=true`        | Enables the complete frontend mock experience |
| `NEXT_PUBLIC_USE_DEMO_CATALOG=true` | Enables the legacy static catalog mode        |

### Demo Mode

With:

```env
NEXT_PUBLIC_DEMO_MODE=true
```

the application provides simulated:

* Authentication
* Cart
* Checkout
* Orders
* Admin REST operations
* Dashboard GraphQL data

Demo data is stored locally in the browser.

### Catalog Fallback

When the API is unavailable and neither demo flag is enabled, the frontend can fall back to static catalog data for browse-only pages.

---

# 🧰 Technology Stack

## Frontend

* Next.js
* React
* TypeScript
* Tailwind CSS
* Redux Toolkit

## Backend

* Node.js
* Express
* TypeScript
* Prisma
* PostgreSQL
* Redis
* Socket.IO

## Payments & Services

* Stripe
* Cloudinary

## Development

* Docker
* Docker Compose
* Swagger
* Prisma Migrations
* Seed Scripts

---

# 🤝 Contributing

Contributions are welcome!

To contribute:

1. Fork the repository
2. Create a new branch
3. Make your changes
4. Test your changes locally
5. Commit your changes
6. Push the branch
7. Open a Pull Request

Please review the contribution guidelines before submitting a PR.

---

# 📄 License

This project is licensed under the **MIT License**.

See the [`LICENSE`](LICENSE) file for more information.
