ADR-002: Implementing Payment Idempotency

Status: Proposed

Project: PayFlow Africa

1. Context

Payment platforms must handle repeated requests, network interruptions, provider timeouts, and uncertain transaction outcomes.

Without proper idempotency controls, repeated requests could result in duplicate financial transactions.

For example, a payment provider may successfully charge a customer while the application fails to receive the provider’s response.

A subsequent customer retry must not automatically create another charge.

2. Decision

Implement durable, atomic idempotency management before initiating payment-provider requests.

The application should:

1. Receive an idempotency key with the payment request.
2. Persist the idempotency claim using a database-enforced uniqueness constraint.
3. Associate the key with a request hash and transaction state.
4. Reuse the existing transaction when the same key and same request payload are received.
5. Reject reuse of the same key with a different request payload.
6. Use payment-provider idempotency capabilities where supported.
7. Resolve uncertain outcomes through provider status inquiries, webhooks, and reconciliation.

3. Alternatives Considered

* Rely entirely on frontend protection against repeated clicks.
* Use Redis as the sole idempotency authority.
* Persist idempotency claims durably in a relational database.

Selected approach: Durable database-backed idempotency, supported by provider idempotency where available.

4. Consequences

Benefits:

* Reduces duplicate-charge risk.
* Supports safe handling of repeated requests.
* Provides durable transaction state.
* Improves payment reconciliation.

Trade-offs:

* Requires careful transaction-state management.
* Introduces additional database operations.
* Requires concurrency and failure testing.
* Provider-specific idempotency behavior must be understood.

5. Failure Handling

If the provider may have processed a payment but the response is lost, the application must not blindly submit a new payment.

The transaction should remain pending or enter a reconciliation state until its outcome is established.

6. Validation

Before implementation, perform:

* Concurrent request testing.
* Duplicate request testing.
* Provider timeout testing.
* Lost-response testing.
* Webhook replay testing.
* Payment reconciliation testing.

7. Conclusion

Durable idempotency is a fundamental reliability control for payment processing. It helps ensure that retries do not unintentionally create duplicate financial transactions.
