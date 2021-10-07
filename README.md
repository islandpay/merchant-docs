
## API endpoints - Merchant

### Authentication

Please request these details from Island Pay

### Subscriptions

#### 1) Get subscriptions

`GET api/merchant/subscriptions`

Response Status:

- `200`: Subscriptions list
- `400`: Unknown error

Response body:

- JSON Array of subscription objects
  - Please refer to **6.1) Subscription**

#### 2) Get subscription transactions

`GET api/merchant/subscriptions/{id}/transactions`

- `id` (`number`) (in URL): Subscription ID

Response Status:

- `200`: Subscription
- `404`: Subscription not found
- `400`: Unknown error

Response body:

- JSON Array of subscription transaction objects
  - Please refer to **6.4) Subscription Transaction**

#### 3) Create subscription

`POST api/merchant/subscriptions`

Request body:

- Please refer to **6.2) Subscription Create Request**

Response Status:

- `200`: Subscription created
- `404`: Customer not found
- `400`: Request error or Unknown error

Response body:

- Please refer to **6.1) Subscription**

#### 4) Change existing subscription

`PUT api/merchant/subscriptions/{id}`

- `id` (`number`) (in URL): Subscription ID

Request body:

- Please refer to **6.3) Subscription Change Request**

Response Status:

- `204`: Subscription changed
- `404`: Subscription not found
- `400`: Request error or Unknown error

#### 5) Cancel Subscription

`DELETE api/merchant/subscriptions/{id}`

- `id` (`number`) (in URL): Subscription ID

Response Status:

- `204`: Subscription canceled
- `404`: Subscription not found
- `400`: Request error or Unknown error

#### 6) Schemas

**6.1) Subscription:**

```json
{
  "id": ___id___,
  "approved": ___approved___,
  "active": ___active___,
  "suspended": ___suspended___,
  "canceled": ___canceled___,
  "start_date": ___start_date___,
  "date_created": ___date_created___,
  "date_approved": ___date_approved___,
  "date_suspended": ___date_suspended___,
  "date_canceled": ___date_canceled___,
  "trial_days": ___trial_days___,
  "user_profile_id": ___user_profile_id___,
  "merchant_name": ___merchant_name___,
  "description": ___description___,
  "amount": ___amount___,
  "subscription_recurrency": ___subscription_recurrency___,
  "consumer_cancel_allowed": ___consumer_cancel_allowed___,
  "sand_dollar": ___sand_dollar___,
  "username": ___username___,
}
```

- `___id___` (`number`): Subscription ID
- `___approved___` (`boolean`): Subscription is 'Approved' (customer approved or subscription was created already approved)
- `___active___` (`boolean`): Subscription is 'Active' (customer will be debited), this will be false when subscription is not approved, is suspended or is canceled
- `___suspended___` (`boolean`): Subscription is 'Suspended' (debit failed)
- `___canceled___` (`boolean`): Subscription is 'Canceled' (by merchant or by customer if allowed through the app)
- `___start_date___` (`string`): Date to start this subscription (ISO 8601 format)
- `___date_created___` (`string`): Date when subscription was created (ISO 8601 format)
- `___date_approved___` (`string`): Date when subscription was approved (ISO 8601 format) (`null` if not approved)
- `___date_suspended___` (`string`): Date when subscription was suspended (ISO 8601 format) (`null` if not suspended)
- `___date_canceled___` (`string`): Date when subscription was canceled (ISO 8601 format) (`null` if not canceled)
- `___trial_days___` (`number`): Trial days of the subscription
- `___user_profile_id___` (`number`): ID of the customer profile
- `___merchant_name___` (`string`): Merchant name shown to the customer
- `___description___` (`string`): Set a description of the subscription (will show on the customer app)
- `___amount___` (`number`): Amount to be debited from customer account
- `___subscription_recurrency___` (`number`): Recurrency of the subscription (check the table bellow with the possible values)
- `___consumer_cancel_allowed___` (`boolean`): Shows a 'cancel' button to the customer on Island Pay app if enabled
- `___sand_dollar___` (`boolean`): Debit the Sand Dollar customer account
- `___username___` (`string`): Customer phone number

**6.2) Subscription Creation Request:**

```json
{
  "user_phone": ___user_phone___,
  "amount": ___amount___,
  "recurrency": ___recurrency___,
  "start_date": ___start_date___,
  "trial_days": ___trial_days___,
  "consumer_cancel_allowed": ___consumer_cancel_allowed___,
  "active": ___active___,
  "description": ___description___,
  "sand_dollar": ___sand_dollar___,
}
```

- `___user_phone___` (`string`): Customer phone number in international format (e.g.: +1 242 000 2233)
- `___amount___` (`number`): Amount to be debited from customer account according to the recurrency
- `___recurrency___` (`number`): Recurrency of the subscription (check the table bellow with the allowed values)
- `___start_date___` (`string`): Date to start this subscription (ISO 8601 format)
- `___trial_days___` (`number`): How many trial days before the charges are applied to customer according to the recurrency
- `___consumer_cancel_allowed___` (`boolean`): Show a 'cancel' button to the customer on Island Pay app
- `___active___` (`boolean`): Start this subscription immediately as active without confirmation by the customer
- `___description___` (`string`): Set a description of the subscription (will show on the customer app)
= `___sand_dollar___` (`boolean`): Debit the Sand Dollar customer account

**6.3) Subscription Change Request:**

```json
{
  "amount": ___amount___,
  "recurrency": ___recurrency___,
  "consumer_cancel_allowed": ___consumer_cancel_allowed___,
  "description": ___description___,
  "sand_dollar": ___sand_dollar___,
}
```

- `___amount___` (`number`): Amount to be debited from customer account according to the recurrency
- `___recurrency___` (`number`): Recurrency of the subscription (check the table bellow with the allowed values)
- `___consumer_cancel_allowed___` (`boolean`): Show a 'cancel' button to the customer on Island Pay app
- `___description___` (`string`): Set a description of the subscription (will show on the customer app)
= `___sand_dollar___` (`boolean`): Debit the Sand Dollar customer account

**6.4) Subscription Transaction:**

```json
{
  "id": ___id___,
  "recurrency_timestamp": ___recurrency_timestamp___,
  "transaction_timestamp": ___transaction_timestamp___,
  "payment_subscription_id": ___payment_subscription_id___,
  "processed_successfully": ___processed_successfully___,
  "customer_transaction_reference": ___customer_transaction_reference___,
  "merchant_transaction_reference": ___merchant_transaction_reference___,
  "reference": ___reference___,
  "payment_amount": ___payment_amount___,
  "subscription_recurrency": ___subscription_recurrency___,
}
```

- `___id___` (`number`): Subscription Transaction ID
- `___recurrency_timestamp___` (`string`): Recurrency date of this transaction (ISO 8601 format) (more details bellow)
- `___transaction_timestamp___` (`string`): Debit date of this transaction (ISO 8601 format)
- `___payment_subscription_id___` (`string`): Subscription ID
- `___processed_successfully___` (`string`): Debit successful
- `___customer_transaction_reference___` (`string`): Customer transaction reference on Island Pay
- `___merchant_transaction_reference___` (`string`): Merchant transaction reference on Island Pay
- `___reference___` (`string`): Unique identifier of this subscription transaction
- `___payment_amount___` (`string`): Debit amount
- `___subscription_recurrency___` (`string`): Subscription recurrency at the time of this transaction (check the table bellow with the possible values)

Note:

The difference between recurrency timestamp and transaction timestamp can be easily explained with an example. If the customer doesn't have enough funds for the debit, the account will be suspended, if on the following day, the customer unsuspends the subscription with enough funds for the debit, the recurrency date will be the date of the first try, the transaction date will be the date of the successful debit

#### 7) Recurrency Types

| `ID` | Description |
| --- | --- |
| `10` | Daily |
| `20` | Weekly |
| `30` | Biweekly |
| `40` | Monthly |
| `50` | Quarterly |
| `60` | Half Yearly |
| `70` | Yearly |
| `100` | Lifetime |
