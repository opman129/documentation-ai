# documentation-ai

Developer documentation for the **Acme Payments API** (v1.4.0): three
hand-written guides and an OpenAPI 3.0.3 specification. The API lets you charge
cards, store customers and payment methods, issue refunds, and subscribe to
webhook events.

This repository holds documentation only. There is no application code, build
step, or test suite.

## Contents

| File | What it covers |
| --- | --- |
| [quickstart.md](./quickstart.md) | Get a token, create a customer, and charge a test card in the sandbox. Includes the test card numbers. |
| [authentication.md](./authentication.md) | Client-credentials tokens, token lifetime, scopes, test and live keys, and `X-API-Key` auth. |
| [errors.md](./errors.md) | The error object, status codes, error types and codes, rate limits, and idempotent retries. |
| [openapi.yaml](./openapi.yaml) | The API reference: endpoints, parameters, schemas, and examples. |

New to the API? Start with the [Quickstart](./quickstart.md).

## API at a glance

### Servers

| Environment | Base URL |
| --- | --- |
| Production | `https://api.example-pay.com/v1` |
| Sandbox (test mode) | `https://sandbox.example-pay.com/v1` |
| Regional production | `https://{region}.api.example-pay.com/v1`, where `region` is `eu` (default), `us`, or `ap` |

### Endpoints

| Method | Path | Description |
| --- | --- | --- |
| `POST` | `/auth/token` | Create an access token |
| `GET` | `/customers` | List customers |
| `POST` | `/customers` | Create a customer |
| `GET` | `/customers/{customerId}` | Retrieve a customer |
| `PATCH` | `/customers/{customerId}` | Update a customer |
| `DELETE` | `/customers/{customerId}` | Delete a customer |
| `GET` | `/payments` | List payments |
| `POST` | `/payments` | Create a payment |
| `GET` | `/payments/{paymentId}` | Retrieve a payment |
| `POST` | `/payments/{paymentId}/capture` | Capture a payment |
| `GET` | `/payments/{paymentId}/refunds` | List refunds for a payment |
| `POST` | `/payments/{paymentId}/refunds` | Refund a payment |
| `POST` | `/payment-methods` | Create a payment method |
| `GET` | `/payment-methods/{paymentMethodId}` | Retrieve a payment method |
| `GET` | `/webhook-endpoints` | List webhook endpoints |
| `POST` | `/webhook-endpoints` | Create a webhook endpoint |

### Conventions

- **Format:** all requests use HTTPS, and all request and response bodies are
  JSON.
- **Authentication:** send a bearer token from `POST /auth/token` on every other
  request. Tokens last one hour and there is no refresh token. See
  [Authentication](./authentication.md).
- **Amounts:** an integer in the currency's minor unit plus an ISO 4217 code, so
  `{ "value": 2000, "currency": "USD" }` means $20.00.
- **Pagination:** list endpoints for customers, payments, and refunds accept
  `limit` (1–100, default 20) and `starting_after`, a cursor set to an object ID.
- **Idempotency:** send an `Idempotency-Key` header when creating payments and
  refunds so retries don't create duplicates.
- **Errors:** failed requests return a non-2xx status and a JSON `error` object.
  Branch on `type` and `code`, not `message`. See
  [Errors and status codes](./errors.md).

## Working on the docs

Treat [openapi.yaml](./openapi.yaml) as the reference for endpoint behaviour, and
keep the guides in step with it when either changes.

You can check the spec with any OpenAPI linter, for example:

```bash
npx @redocly/cli lint openapi.yaml
```

Before publishing, remove the platform note in the HTML comment in
[authentication.md](./authentication.md). It explains how to embed the
`POST /auth/token` operation on Documentation.AI, Mintlify, or GitBook.

Never commit real credentials. Examples should use placeholders such as
`sk_test_YOUR_SECRET`.

## Support

Contact Acme Developer Support at <api@example-pay.com> or through the
[support page](https://docs.example-pay.com/support). The API is proprietary; see
the [license](https://docs.example-pay.com/license) and
[terms of service](https://docs.example-pay.com/terms).
