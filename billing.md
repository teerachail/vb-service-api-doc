# Billing Agreements API

After you create and activate a billing plan, you can use the /billing-agreements resource to create billing agreements. A buyer must approve an agreement before you can execute it. See Billing Agreements.

## Prerequisites

Before you can use the Billing Agreements API, you must make your first call and learn about REST API authentication and headers.

Then, become familiar with basic payment processes, such as how to accept credit card payments.

If you are a non-US developer, see International Developer Questions.

## Billing agreements (resource group)

Use the `/billing-agreements` resource to:

* Create agreement
* Update agreement
* Show agreement details
* Bill agreement balance
* Cancel agreement
* Re-activate agreement
* Set agreement balance
* Suspend agreement
* List agreement transactions
* Execute agreement

## Create billing agreement

Use the following call to create a billing agreement. The response for this call includes these HATEOAS links: an approval_url link and an execute link. Each returned link includes the token for the agreement.

> Note: For PayPal payments, after creating the agreement, you need to get approval for the billing agreement and then execute the billing agreement. Billing agreements for credit card payments execute automatically when created so there is no need for the user to approve the agreement or to execute the agreement.

```
curl -v -X POST https://api.sandbox.vb.com/billing-agreements/ \
-H "Content-Type:application/json" \
-H "Authorization: Bearer Access-Token" \
-d '{
  "name": "Override Agreement",
  "description": "Agreement where merchant preferences and charge model is overridden",
  "start_date": "2016-11-22T20:53:43Z",
  "payer": {
  "payment_method": "paypal",
  "payer_info": {
    "email": "kkarunanidhi-per1@paypal.com"
  }
  },
  "plan": {
  "id": "P-1WJ68935LL406420PUTENA2I"
  },
  "shipping_address": {
  "line1": "Hotel Staybridge",
  "line2": "Crooke Street",
  "city": "San Jose",
  "state": "CA",
  "postal_code": "95112",
  "country_code": "US"
  },
  "override_merchant_preferences": {
  "setup_fee": {
    "value": "3",
    "currency": "GBP"
  },
  "return_url": "http://indiatimes.com",
  "cancel_url": "http://rediff.com",
  "auto_bill_amount": "YES",
  "initial_fail_amount_action": "CONTINUE",
  "max_fail_attempts": "11"
  },
  "override_charge_models": [
  {
    "charge_id": "CHM-8373958130821962WUTENA2Q",
    "amount": {
    "value": "1",
    "currency": "GBP"
    }
  }
  ]
}'
```

A successful call returns an agreement object that is based on the billing plan. The agreement object includes an agreement id and details.

### Response

```
{
  "name": "Override Agreement",
  "description": "Agreement where merchant preferences and charge model is overridden",
  "plan": {
    "id": "P-1WJ68935LL406420PUTENA2I",
    "state": "ACTIVE",
    "name": "Fast Speed Plan",
    "description": "Vanilla plan",
    "type": "INFINITE",
    "payment_definitions": [
      {
        "id": "PD-9WG6983719571780GUTENA2I",
        "name": "Payment Definition-1",
        "type": "REGULAR",
        "frequency": "Day",
        "amount": {
          "currency": "GBP",
          "value": "10"
        },
        "charge_models": [
          {
            "id": "CHM-8373958130821962WUTENA2Q",
            "type": "SHIPPING",
            "amount": {
              "currency": "GBP",
              "value": "1"
            }
          },
          {
            "id": "CHM-2937144979861454NUTENA2Q",
            "type": "TAX",
            "amount": {
              "currency": "GBP",
              "value": "2"
            }
          }
        ],
        "cycles": "0",
        "frequency_interval": "1"
      },
      {
        "id": "PD-89M493313S710490TUTENA2Q",
        "name": "Payment Definition-1",
        "type": "TRIAL",
        "frequency": "Month",
        "amount": {
          "currency": "GBP",
          "value": "100"
        },
        "charge_models": [
          {
            "id": "CHM-78K47820SS4923826UTENA2Q",
            "type": "SHIPPING",
            "amount": {
              "currency": "GBP",
              "value": "10"
            }
          },
          {
            "id": "CHM-9M366179U7339472RUTENA2Q",
            "type": "TAX",
            "amount": {
              "currency": "GBP",
              "value": "12"
            }
          }
        ],
        "cycles": "5",
        "frequency_interval": "2"
      }
    ],
    "merchant_preferences": {
      "setup_fee": {
        "currency": "GBP",
        "value": "3"
      },
      "max_fail_attempts": "11",
      "return_url": "http://indiatimes.com",
      "cancel_url": "http://rediff.com",
      "auto_bill_amount": "YES",
      "initial_fail_amount_action": "CONTINUE"
    }
  },
  "links": [
    {
      "href": "https://stage2p2163.qa.paypal.com/cgi-bin/webscr?cmd=_express-checkout&token=EC-83C745436S346813F",
      "rel": "approval_url",
      "method": "REDIRECT"
    },
    {
      "href": "https://stage2p2163.qa.paypal.com/v1/payments/billing-agreements/EC-83C745436S346813F/agreement-execute",
      "rel": "execute",
      "method": "POST"
    }
  ],
  "start_date": "2016-11-22T20:53:43Z"
}
```

## Get approval for billing agreement

Note the HATEOAS links in the preceding sample response. For PayPal payments, direct the user to the approval_url on the PayPal site so that the user can approve the agreement. The user must approve the agreement before you can execute it.

When the buyer clicks the approval URL for the agreement, you obtain buyer details.

For PayPal payments, you must call the execute link to execute the billing agreement.

> Note: For credit card payments, the agreement is automatically executed when created.

```
curl -v -X POST https://api.sandbox.vb.com/billing-agreements/EC-6CT996018D989343F/agreement-execute \
-H "Content-Type:application/json" \
-H "Authorization: Bearer Access-Token"
```

A successful call returns agreement details:

```
{
  "id": "I-0P92RHPVX2T4",
  "state": "Pending",
  "description": "Agreement for Fast Speed Plan",
  "payer": {
    "payment_method": "paypal",
    "status": "verified",
    "payer_info": {
      "email": "test113@113.com",
      "first_name": "Tester",
      "last_name": "Testqaz",
      "payer_id": "YP2KV9HJ4F7C4",
      "shipping_address": {
        "recipient_name": "Tester Testqaz",
        "line1": "StayBr111idge Suites",
        "line2": "Cro12ok Street",
        "city": "San Jose",
        "state": "CA",
        "postal_code": "95112",
        "country_code": "US"
      }
    }
  },
  "plan": {
    "payment_definitions": [
      {
        "type": "TRIAL",
        "frequency": "Month",
        "amount": {
          "value": "20.00"
        },
        "cycles": "4",
        "charge_models": [
          {
            "type": "TAX",
            "amount": {
              "value": "20.00"
            }
          },
          {
            "type": "SHIPPING",
            "amount": {
              "value": "10.60"
            }
          }
        ],
        "frequency_interval": "1"
      },
      {
        "type": "REGULAR",
        "frequency": "Month",
        "amount": {
          "value": "100.00"
        },
        "cycles": "0",
        "charge_models": [
          {
            "type": "TAX",
            "amount": {
              "value": "20.00"
            }
          },
          {
            "type": "SHIPPING",
            "amount": {
              "value": "10.60"
            }
          }
        ],
        "frequency_interval": "1"
      }
    ],
    "merchant_preferences": {
      "setup_fee": {
        "value": "25.00"
      },
      "max_fail_attempts": "1",
      "auto_bill_amount": "YES"
    },
    "links": [],
    "currency_code": "USD"
  },
  "links": [
    {
      "href": "https://api.sandbox.vb.com/v1/payments/billing-agreements/I-0P92RHPVX2T4",
      "rel": "self",
      "method": "GET"
    }
  ],
  "start_date": "2016-11-22T20:53:43Z",
  "shipping_address": {
    "recipient_name": "Tester Testqaz",
    "line1": "StayBr111idge Suites",
    "line2": "Cro12ok Street",
    "city": "San Jose",
    "state": "CA",
    "postal_code": "95112",
    "country_code": "US"
  },
  "agreement_details": {
    "outstanding_balance": {
      "value": "0.00"
    },
    "cycles_remaining": "4",
    "cycles_completed": "0",
    "final_payment_date": "2016-12-22T20:53:43Z",
    "failed_payment_count": "0"
  }
}
```

