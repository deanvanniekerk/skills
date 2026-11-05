# Example PR body excerpt

This is a fictional example, not a record of a real run.

## Request

> /spec-review on spec #40 (order confirmation), merged as #41–#44

## Consolidations

- **One address validator.** #41 added `validateDeliveryAddress` in `orders/address.ts`; #43 added `isAddressComplete` in `checkout/confirm.ts`, with the same required fields in another order. Both now call the existing `addressSchema` in `shared/address.ts`, and both helpers are gone (−84 lines). Covered by the existing schema tests and the two confirmation tests.
- **Status labels from one map.** #42 and #44 each wrote a `switch` from order status to label. Both read `orderStatusLabels` now (−41 lines).

## Harmonised

- `useConfirmOrder` (#43) fetched with its own `fetch` wrapper; it now uses the codebase's `useResourceMutation`, like every other write.

## Bugs fixed

- `orders/cancel.ts:57`: a partial refund from #44 left an order Confirmed after cancelling, because #41's cancel path never saw the new refund state. Cancel now reads the shared status rule. Test: "cancels an order holding a partial refund".

## Open questions

- **Cancel after a partial refund.** Should the refund be reversed, or kept? Scenario: a customer is refunded €20 of €100, then cancels. #44 says nothing about it.

## Discarded

- `OrderCard` and `OrderRow` look alike, but they render different fields for different screens; merging them would add a mode flag.
