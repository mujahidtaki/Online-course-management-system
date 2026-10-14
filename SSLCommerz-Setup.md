# SSLCommerz setup for CourseHub

CourseHub uses the hosted SSLCOMMERZ V4 flow from the supplied ASP.NET integration sample, adapted to ASP.NET Core MVC.

## Flow

1. CourseHub creates a unique `tran_id`.
2. CourseHub sends customer/order data to the SSLCOMMERZ session API.
3. The browser is redirected to `GatewayPageURL`.
4. SSLCOMMERZ posts the result to `PaymentCallback`.
5. CourseHub validates the transaction server-side using `val_id`.
6. SSLCOMMERZ IPN is accepted at `PaymentIpn`.
7. IPN signatures are verified and the transaction is validated again.
8. CourseHub activates the enrollment only after amount, currency and transaction ID match.
9. Risk level `1` is held for manual review instead of automatically activating the course.

## Local development

When `SslCommerz:CallbackBaseUrl` is empty and CourseHub is running in Development, the hosted checkout uses the current application origin such as `http://localhost:5000` for the browser return URLs. This lets you test the hosted sandbox checkout from the same PC.

IPN is server-to-server, so a plain localhost URL is not reachable by SSLCOMMERZ. For IPN testing, expose the application through a public HTTPS tunnel and set `SslCommerz:CallbackBaseUrl`.

The development-only `Local test` button is also available for testing the enrollment/payment database workflow without an external gateway callback.

For production or public sandbox testing, set:

```json
"SslCommerz": {
  "StoreId": "YOUR_SANDBOX_STORE_ID",
  "StorePassword": "YOUR_SANDBOX_STORE_PASSWORD",
  "UseSandbox": true,
  "CallbackBaseUrl": "https://YOUR-PUBLIC-HTTPS-HOST"
}
```

The callback URLs are:

- `/Student/PaymentCallback`
- `/Student/PaymentFailed`
- `/Student/PaymentCancelled`
- `/Student/PaymentIpn`

Configure the IPN listener in the SSLCOMMERZ merchant/sandbox panel to the public `/Student/PaymentIpn` URL.

Never commit live Store Password values to source control.

## Supplied sample

The uploaded `SSLCommerz-ASP.NET-master.zip` is an older ASP.NET Web Forms sample. CourseHub does not copy its Web Forms pages. It ports the relevant V4 hosted-payment concepts into an ASP.NET Core MVC service so the payment code is testable, isolated and compatible with the CourseHub architecture.
