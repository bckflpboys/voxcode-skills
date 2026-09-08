# Example Walkthrough: Payment & Checkout Flow

This example demonstrates how an AI agent uses the **Flow Simulation** skill to diagnose and fix a race condition between an e-commerce checkout session and an external payment webhook.

---

## The User Prompt
> *"Some customers say their credit cards were charged, but their orders are stuck in 'Pending Payment' in our dashboard, and they didn't receive an order confirmation email."*

---

## ❌ Shallow AI Failure Mode
The shallow AI looks at the orders database table, finds the query where `status = 'Pending Payment'`, and changes a status setter or adds a manual "Fix Status" button in the admin panel. The root-cause race condition remains active, continuing to affect paying customers.

---

##  Flow Simulation in Action

### Phase 1: Route & Pipeline Discovery
The AI traces both the user journey and the asynchronous webhook lifecycle:

```
[Customer Browser]
       │ (Submits Cart)
       ▼
[POST /api/checkout/session] -> Creates Stripe Checkout Session
       │                         Saves Order (Status: 'Awaiting_Payment')
       ▼
[Stripe Hosted Checkout] -> Customer enters card -> Stripe charges card
       │
       ├──────────────────────────────────────────────────────┐
       │ (Browser Redirect)                                   │ (Asynchronous Webhook)
       ▼                                                      ▼
[GET /checkout/success?session_id=cs_123]            [POST /api/webhooks/stripe]
       │                                                      │
[Fulfillment Check in DB]                               [Verify Stripe Signature]
       │                                                      │
[If status == 'Paid', show receipt]                    [Update Order Status -> 'Paid']
                                                              │
                                                        [Send Confirmation Email]
```

---

### Phase 2: Blast Radius & Interaction Mapping
- **Webhooks**: Stripe webhooks are asynchronous and can arrive *after* or *concurrently* with the user's browser redirect to `/checkout/success`.
- **Database Model**: `orders` table has `id`, `stripe_session_id`, `payment_status`, `items`.
- **Email Service**: Dispatches customer confirmation receipt.
- **Inventory Service**: Decrements stock once an order is marked `'Paid'`.

---

### Phase 3: Pre-Fix Flow Simulation

The AI steps through the timing execution:
1. Fast User Redirect Scenario:
   - Stripe redirects customer browser to `/checkout/success?session_id=cs_123` within 200ms.
   - Browser makes request to check order status.
   - At that exact millisecond, Stripe's webhook has not yet finished delivering to `/api/webhooks/stripe`.
   - The success page queries the DB: `status` is still `'Awaiting_Payment'`.
   - The page displays an unhandled error: *"Order not found or payment pending."*
2. Duplicate Webhook Scenario:
   - Stripe retries webhooks if the server response takes > 3 seconds.
   - Without an idempotency check, the webhook handler attempts to decrement inventory twice!

---

### Phase 4: Implementation
1. **Webhook Idempotency**:
   - Store processed Stripe event IDs in an `idempotency_keys` table.
   - If event was already processed, immediately return HTTP 200 without duplicate execution.
2. **Success Page Verification Polling / Synchronous Fallback**:
   - On the `/checkout/success` page, if the order is still `'Awaiting_Payment'`, the server queries the Stripe API directly (`stripe.checkout.sessions.retrieve(sessionId)`) as a fallback verification.
   - If Stripe reports `payment_status === 'paid'`, execute the order completion logic within a mutex/transaction.
3. **Atomic Stock Decrement**:
   - Ensure stock is decremented strictly once using atomic SQL updates:
     `UPDATE inventory SET stock = stock - qty WHERE id = ... AND stock >= qty`.

---

### Phase 5: Post-Fix Simulation & Verification
1. Simulated customer arriving at `/checkout/success` before the webhook arrives -> synchronous fallback handles settlement cleanly.
2. Simulated duplicate webhook delivery -> idempotency filter recognizes duplicate and returns 200 OK without double decrementing inventory.
3. Verified adjacent flows: Cart abandonment, refund webhooks, and manual admin fulfillment.
