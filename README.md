# CourseHub

CourseHub is an ASP.NET Core MVC 10 learning platform designed for Visual Studio and XAMPP MariaDB/MySQL.

## Stack

- ASP.NET Core 10 MVC
- C#
- Entity Framework Core 10
- Pomelo MySQL provider
- MariaDB/MySQL from XAMPP
- Cookie authentication
- BCrypt password hashing
- SSLCommerz hosted checkout, validation and IPN
- Responsive 3D/glassmorphism UI

## Run locally

1. Start MySQL in XAMPP. Default port: `3306`.
2. Open `CourseHub.csproj` in Visual Studio.
3. Confirm `appsettings.json` matches your XAMPP MySQL username/password/port.
4. Build Solution.
5. Press F5.
6. Open `http://localhost:5000`.

The app creates the `coursehub` database and seed data on first run.

## Demo accounts

- Admin: `admin@coursehub.com` / `admin123`
- Teacher: `sarah@coursehub.com` / `teacher123`
- Student: `alice@student.com` / `student123`

## SSLCommerz

The project uses the official SSLCOMMERZ V4 flow: create session, redirect to hosted checkout, receive callback/IPN, and validate the transaction server-side.

For real gateway checkout, set:

- `SslCommerz:StoreId`
- `SslCommerz:StorePassword`
- `SslCommerz:UseSandbox`
- `SslCommerz:CallbackBaseUrl`

`CallbackBaseUrl` must be a public HTTPS address reachable by SSLCOMMERZ for IPN/callback delivery. `localhost` cannot receive gateway-to-server IPN messages.

The default `testbox` / `qwerty` values are only the public sandbox sample credentials from the supplied SSLCOMMERZ ASP.NET sample. Replace them with your own sandbox store credentials when required.

## Important

This repository does not include production secrets. Do not commit live Store ID/password values.
