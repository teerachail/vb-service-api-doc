# Invoice API

Use the REST Invoicing API to create draft invoices and send and manage invoices.

When you send an invoice, the invoice moves from draft to payable state and PayPal emails a link to the invoice on the PayPal website to the customer. The customer can view the invoice on the PayPal website and pay with PayPal, a debit card, or a credit card. Or, you can record off-line payments, such as a check.

## Prerequisites

To get started, create a REST API app. After you make your first call to the REST API, review the Invoicing API reference.

To call the Invoicing API on behalf of a merchant, you must obtain third-party permissions:

Set up Log In with PayPal so that the merchant can authorize you. To do so, get the Invoicing scope. Then, Integrate Log In with PayPal.

The merchant uses your app to Log In with PayPal and authorizes you.

After you are authorized, you can call any Invoicing API on behalf of the merchant.

## Invoices (resource group)

Use the `/invoicing/invoices` resource to create, generate a QR code for, list, search for, show details for, update, send, and send a reminder for invoices.

You can also generate an invoice number, delete draft invoices, and cancel sent invoices.

To manage payments, you can mark an invoice as partially or fully paid or refunded and delete an external payment or an external refund from an invoice.

## Create a draft invoice

You can create a draft invoice directly or from a template. To create an invoice directly, you must include invoice details including merchant information in the JSON request body. The invoice object must include an items array with line item information.

The following example shows you how to create an invoice directly.

To create a draft invoice directly, specify request parameters in the JSON request body, as shown here:

```
curl -v -X POST https://api.sandbox.vb.com/v1/invoicing/invoices/ \
-H "Content-Type:application/json" \
-H "Authorization: Bearer Access-Token" \
-d '{
  "merchant_info": {
    "email": "ppaas_default@paypal.com",
    "first_name": "Dennis",
    "last_name": "Doctor",
    "business_name": "Medical Professionals, LLC",
    "phone": {
      "country_code": "001",
      "national_number": "5032141716"
    },
    "address": {
      "line1": "1234 Main St.",
      "city": "Portland",
      "state": "OR",
      "postal_code": "97217",
      "country_code": "US"
    }
  },
  "billing_info": [{
    "email": "example@example.com"
  }],
  "items": [{
    "name": "Sutures",
    "quantity": 100,
    "unit_price": {
      "currency": "USD",
      "value": "5"
    }
  }],
  "note": "Medical Invoice 16 Jul, 2013 PST",
  "payment_term": {
    "term_type": "NET_45"
  },
  "shipping_info": {
    "first_name": "Sally",
    "last_name": "Patient",
    "business_name": "Not applicable",
    "phone": {
      "country_code": "001",
      "national_number": "5039871234"
    },
    "address": {
      "line1": "1234 Broad St.",
      "city": "Portland",
      "state": "OR",
      "postal_code": "97216",
      "country_code": "US"
    }
  }
}'
```

A successful call returns an invoice object that includes the ID of the created invoice:

```
{
  "id": "INV2-RUVR-ADWQ-H89Y-ABCD",
  "number": "ABCD4971",
  "status": "DRAFT",
  "merchant_info": {
    "email": "ppaas_default@paypal.com",
    "first_name": "Dennis",
    "last_name": "Doctor",
    "business_name": "Medical Professionals, LLC",
    "phone": {
      "country_code": "1",
      "national_number": "5032141234"
    },
    "address": {
      "line1": "1234 Main St.",
      "city": "Portland",
      "state": "OR",
      "postal_code": "97217",
      "country_code": "US"
    }
  },
  "billing_info": [
    {
      "email": "email@example.com"
    }
  ],
  "shipping_info": {
    "first_name": "Sally",
    "last_name": "Patient",
    "business_name": "Not applicable",
    "phone": {
      "country_code": "1",
      "national_number": "5039871234"
    },
    "address": {
      "line1": "1234 Broad St.",
      "city": "Portland",
      "state": "OR",
      "postal_code": "97216",
      "country_code": "US"
    }
  },
  "items": [
    {
      "name": "Sutures",
      "quantity": 100,
      "unit_price": {
        "currency": "USD",
        "value": "5.00"
      }
    }
  ],
  "invoice_date": "2014-02-27 PST",
  "payment_term": {
    "term_type": "NET_45",
    "due_date": "2014-04-13 PDT"
  },
  "tax_calculated_after_discount": false,
  "tax_inclusive": false,
  "note": "Medical Invoice 16 Jul, 2013 PST",
  "total_amount": {
    "currency": "USD",
    "value": "500.00"
  }
}
```

Next, send the invoice. When you send an invoice, it moves from a draft to payable state.

## Send an invoice

When you send an invoice, PayPal sends an email to the customer with a link to the invoice on the PayPal site. The customer clicks the link and reviews and pays the invoice.

To send an invoice to a customer, specify the invoice ID in the URI of the request. By default, the notify_merchant query parameter is set to true, which sends the merchant an invoice update notification. To withhold this notification, set this parameter to false.

```
curl -v -X POST https://api.sandbox.vb.com/v1/invoicing/invoices/INV2-EHNV-LJ5S-A7DZ-V6NJ/send \
  -H "Content-Type:application/json" \
  -H "Authorization: Bearer Access-Token"
```

A successful call returns the HTTP `202 (Accepted)` status code. After you send an invoice, it changes from draft to payable state.

Next, you can show invoice details. Your app can display these details to the customer.

## Manage payments

To manage payments, you can mark an invoice as partially or fully paid or refunded. You can also delete an external payment or an external refund from an invoice.

To complete these tasks for payments, use the /invoicing/invoices resource: