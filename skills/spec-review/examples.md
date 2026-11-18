# Example findings file excerpt

This is a fictional example, not a record of a real run.

## Request

> /spec-review on spec #40 (order confirmation), merged as #41–#44

## Findings

### F1 · consolidation · one address validator

- **Sites:** `orders/address.ts:12` (`validateDeliveryAddress`, #41), `checkout/confirm.ts:88` (`isAddressComplete`, #43), `shared/address.ts:5` (`addressSchema`).
- **Wrong:** both helpers check the same required fields, in a different order, beside the existing `addressSchema`.
- **Move:** both callers use `addressSchema`; delete both helpers.
- **Size:** −84 lines, two concepts.
- **Verified:** traced both callers; the schema tests and the two confirmation tests cover the fields.

### F2 · bug · cancel ignores a partial refund

- **Sites:** `orders/cancel.ts:57`, `orders/refund.ts:30` (#44).
- **Wrong:** an order holding a partial refund stays Confirmed after cancelling, because #41's cancel path never sees #44's refund state.
- **Move:** cancel reads the shared status rule in `orders/status.ts`.
- **Size:** −6 lines.
- **Verified:** failing test "cancels an order holding a partial refund":

  ```ts
  const order = await confirmedOrder({ refunded: 20_00 });
  await cancelOrder(order.id);
  expect((await getOrder(order.id)).status).toBe("cancelled");
  ```

## Open questions

- **Cancel after a partial refund.** Should the refund be reversed, or kept? Scenario: a customer is refunded €20 of €100, then cancels. #44 says nothing about it.

## Discarded

- `OrderCard` and `OrderRow` look alike, but they render different fields for different screens; merging them would add a mode flag.
