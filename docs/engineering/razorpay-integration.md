# 💳 Engineering: Razorpay Payment Gateway Integration

This document outlines the server-side and client-side integration of the **Razorpay Payment Gateway API** within Medilab.

---

## 🏗️ Architecture & SDK Setup

- **Package**: `razorpay/razorpay: ^2.8`
- **Controller**: `App\Http\Controllers\RazorpayPaymentController`
- **Configuration**:
  ```env
  RAZORPAY_KEY=rzp_test_xxxxxx
  RAZORPAY_SECRET=xxxxxx
  ```

---

## 🔄 Transaction Flow & Capture

```mermaid
sequenceDiagram
    autonumber
    actor Patient as Patient Browser
    participant Controller as RazorpayPaymentController
    participant RazorpaySDK as Razorpay PHP API SDK
    participant Gateway as Razorpay Servers

    Patient->>Controller: GET /razorpay-payment
    Controller-->>Patient: Render razorpayView.blade.php with Key & Amount
    Patient->>Gateway: Submit Payment (Card / UPI / NetBanking)
    Gateway-->>Patient: Returns razorpay_payment_id
    Patient->>Controller: POST /razorpay-payment with razorpay_payment_id
    Controller->>RazorpaySDK: new Api(KEY, SECRET)
    Controller->>RazorpaySDK: fetch(razorpay_payment_id)->capture(['amount' => $amount])
    RazorpaySDK->>Gateway: Verify Signature & Capture Funds
    Gateway-->>RazorpaySDK: Capture Success Response
    RazorpaySDK-->>Controller: Confirmed Transaction
    Controller-->>Patient: Redirect with Success Flash Message
```

---

## 🛡️ Error Handling & Idempotency
- All capture invocations are enclosed in `try ... catch (Exception $e)` blocks.
- Failed captures flash user-friendly error messages and redirect gracefully back to the checkout view without crashing the session.
