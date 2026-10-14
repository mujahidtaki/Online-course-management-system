# SSLCommerz and invoice update

## Hosted SSLCommerz checkout

The Student > My Payments page now starts the supplied SSLCOMMERZ V4 hosted checkout flow.

The flow is:

1. CourseHub creates a unique transaction.
2. `Student/PayNow` sends the transaction to `https://sandbox.sslcommerz.com/gwprocess/v4/api.php` in sandbox mode.
3. CourseHub reads `GatewayPageURL` from the response and redirects the browser there.
4. SSLCOMMERZ posts the result to `Student/PaymentCallback`.
5. CourseHub calls the SSLCOMMERZ validation API and verifies transaction ID, amount and currency before activating enrollment.
6. `Student/PaymentIpn` verifies the IPN signature and validates the transaction again when an IPN reaches the application.
7. Failed/cancelled transactions are recorded without activating the enrollment.

SSLCOMMERZ's official documentation describes the same hosted flow and requires server-side order validation. See the developer documentation:
https://developer.sslcommerz.com/doc/v4/index.html

## Local XAMPP / Visual Studio testing

When `SslCommerz:CallbackBaseUrl` is empty and the application is running in Development, CourseHub automatically uses the current request origin, for example:

`http://localhost:5000`

This lets the browser return to the local ASP.NET application after sandbox checkout.

For reliable IPN delivery and production, set:

```json
"SslCommerz": {
  "StoreId": "YOUR_STORE_ID",
  "StorePassword": "YOUR_STORE_PASSWORD",
  "UseSandbox": true,
  "CallbackBaseUrl": "https://YOUR-PUBLIC-HTTPS-HOST"
}
```

The IPN URL is:

`https://YOUR-PUBLIC-HTTPS-HOST/Student/PaymentIpn`

For local development, the Local test button remains available so the complete CourseHub enrollment/payment database workflow can be tested without relying on an externally reachable IPN endpoint.

## Downloadable invoice

Completed payments now show **Download invoice** in Student > My Payments.

The invoice is generated server-side as a PDF and includes:

- Invoice number
- Payment date
- Student name and email
- Course
- Amount and currency
- Payment method
- CourseHub transaction ID
- SSLCommerz validation ID when available
- Bank transaction ID when available
- Card/gateway type when available
- Completed payment status

The invoice action only allows the currently authenticated student to download their own completed payment invoice.
