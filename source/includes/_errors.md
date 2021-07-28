# Errors

> &nbsp;

```ruby
require "taxjar"
client = Taxjar::Client.new(api_key: "9e0cd62a22f451701f29c3bde214")

begin
  # Invalid request
  order = client.create_order({
    :transaction_date => '2015/05/14',
    :to_country => 'US',
    :to_state => 'CA',
    :to_zip => '90002',
    :amount => 17.45,
    :shipping => 1.5,
    :sales_tax => 0.95
  })
rescue Taxjar::Error => e
  # <Taxjar::Error::NotAcceptable: transaction_id is missing>
  puts e.class.name
  puts e.message
end
```

```python
import taxjar
client = taxjar.Client(api_key='9e0cd62a22f451701f29c3bde214')

try:
  order = client.create_order({
    'transaction_date': '2015/05/14',
    'to_country': 'US',
    'to_state': 'CA',
    'to_zip': '90002',
    'amount': 17.45,
    'shipping': 1.5,
    'sales_tax': 0.95
  })
except taxjar.exceptions.TaxJarConnectionError as err:
  print err
except taxjar.exceptions.TaxJarResponseError as err:
  # 406 Not Acceptable – transaction_id is missing
  print err.full_response
```

```javascript
const Taxjar = require('taxjar');

const client = new Taxjar({
  apiKey: '9e0cd62a22f451701f29c3bde214'
});

// Invalid request
client.createOrder({
  transaction_date: '2015/05/14',
  to_country: 'US',
  to_state: 'CA',
  to_zip: '90002',
  amount: 17.45,
  shipping: 1.5,
  sales_tax: 0.95
}).then(res => {
  res.order; // Order object
}).catch(err => {
  err.detail; // Error detail
  err.status; // Error status code
});
```

```php?start_inline=1
require __DIR__ . '/vendor/autoload.php';
$client = TaxJar\Client::withApiKey("9e0cd62a22f451701f29c3bde214");

try {
  // Invalid request
  $order = $client->createOrder([
    'transaction_date' => '2015/05/14',
    'to_country' => 'US',
    'to_zip' => '90002',
    'to_state' => 'CA',
    'amount' => 17.45,
    'shipping' => 1.5,
    'sales_tax' => 0.95
  ]);
} catch (TaxJar\Exception $e) {
  // 406 Not Acceptable – transaction_id is missing
  echo $e->getMessage();

  // 406
  echo $e->getStatusCode();
}
```

```csharp
using Taxjar;
var client = new TaxjarApi("9e0cd62a22f451701f29c3bde214");

try
{
  // Invalid request
  var order = client.CreateOrder(new {
    transaction_date = "2015/05/04",
    to_country = "US",
    to_zip = "90002",
    to_state = "CA",
    amount = 17.45,
    shipping = 1.5,
    sales_tax = 0.95
  });
}
catch(TaxjarException e)
{
  // 406 Not Acceptable – transaction_id is missing
  e.TaxjarError.Error;
  e.TaxjarError.Detail;
  e.TaxjarError.StatusCode;
}
```

```java
import com.taxjar.Taxjar;
import com.taxjar.exception.TaxjarException;
import com.taxjar.model.transactions.OrderResponse;
import java.util.HashMap;
import java.util.Map;

public class ErrorHandlingExample {

    public static void main(String[] args) {
        Taxjar client = new Taxjar("9e0cd62a22f451701f29c3bde214");

        try {
            Map<String, Object> params = new HashMap<>();
            params.put("transaction_date", "2015/05/04");
            params.put("to_country", "US");
            params.put("to_zip", "90002");
            params.put("to_state", "CA");
            params.put("amount", 17.45);
            params.put("shipping", 1.5);
            params.put("sales_tax", 0.95);

            OrderResponse res = client.createOrder(params);
        } catch (TaxjarException e) {
            // 406 Not Acceptable – transaction_id is missing
            e.getMessage();
            e.getStatusCode();
            e.printStackTrace();
        }
    }

}
```

```go
package main

import (
    "fmt"
    "os"

    "github.com/pkg/errors"
    "github.com/taxjar/taxjar-go"
)

func main() {
    client := taxjar.NewClient(taxjar.Config{
        APIKey: os.Getenv("TAXJAR_API_KEY"),
    })

    res, err := client.CreateOrder(taxjar.CreateOrderParams{
        TransactionDate: "2015/05/04",
        ToCountry:       "US",
        ToZip:           "90002",
        ToState:         "CA",
        Amount:          17.45,
        Shipping:        1.5,
        SalesTax:        0.95,
    })
    if err != nil {
        fmt.Println(err) // taxjar: 406 Not Acceptable - transaction_id is missing
    } else {
        fmt.Println(res.Order)
    }
    // or extract more information by asserting to `*taxjar.Error`
    if err := err.(*taxjar.Error); err != nil {
        fmt.Println(err.Status) // 406
        fmt.Println(err.Err) // Not Acceptable
        fmt.Println(err.Detail) // transaction_id is missing
        fmt.Printf("%+v", errors.Wrap(err, "")) // Stack trace:
        // taxjar: 406 Not Acceptable - transaction_id is missing
        //
        // main.main
        //         /Path/to/your/file.go:185
        // runtime.main
        //         /usr/local/go/src/runtime/proc.go:200
        // runtime.goexit
        //         /usr/local/go/src/runtime/asm_amd64.s:1337
    } else {
        fmt.Println(res.Order)
    }
}
```

The TaxJar API uses the following error codes:

Code | Error Message
---- | ----------------------------------------------------------------------------------
400  | Bad Request -- Your request format is bad.
401  | Unauthorized -- Your API key is wrong.
403  | Forbidden -- The resource requested is not authorized for use.
404  | Not Found -- The specified resource could not be found.
405  | Method Not Allowed -- You tried to access a resource with an invalid method.
406  | Not Acceptable -- Your request is not acceptable.
410  | Gone -- The resource requested has been removed from our servers.
422  | Unprocessable Entity -- Your request could not be processed.
429  | Too Many Requests -- You're requesting too many resources! Slow down!
500  | Internal Server Error -- We had a problem with our server. Try again later.
503  | Service Unavailable -- We're temporarily offline for maintenance. Try again later.

## 400 Bad Request

If you receive the message "No from address, no nexus address, and no address on file", we recommend providing `from_` or `nexus_addresses[]` depending on the endpoint's accepted parameters. Otherwise [sign in](https://app.taxjar.com/sign_in) and provide your [business address and locations](https://app.taxjar.com/account) in TaxJar.

Additionally, we provide specific 400 error messages for invalid data:

- Invalid ZIP and state combinations for `to_zip`, `to_state` and `from_zip`, `from_state`

There are additional scenarios in which `/v2/taxes` may return a 400 error code:

- No `amount` or `line_items[]` are provided
- Non-unique `line_items[][id]`'s
- Missing `to_zip` when `to_country` is `"US"`
- Missing `to_state` when `to_country` is `"US"` or `"CA"`
- `to_zip` isn't a valid postal code code when `to_country` is `"US"`
- Unsupported country codes

### Transactions

When using the `/v2/transactions` endpoints, we return the following error messages:

- `amount` param does not equal sum of `line_items` + `shipping`
- `line_items[][description]` param exceeds limit of 255 characters

## 401 Unauthorized

Verify your API token is correct and make sure you're correctly setting the [Authorization header](#authentication).

## 403 Forbidden

Make sure you have an active API token with TaxJar by [reviewing your account](https://app.taxjar.com/account). Your trial or subscription may have expired. If you're still not sure what's wrong, [contact us](https://www.taxjar.com/contact) and we'll investigate.

## 406 Not Acceptable

This error occurs most often when posting form-encoded data with parameters such as `nexus_addresses[]` or `line_items[]`. If your code looks correct, try submitting the request with a `Content-Type: application/json` header. **Data should always be JSON-encoded.**

Additionally, a 406 response will be returned if you provide blank values for required fields when pushing orders or refunds through the `/v2/transactions` endpoints.

Additional 406 error response reasons:

- State or country is not a valid two-letter ISO code
- Shipping or another required field is missing
- Shipping or another field has an invalid value using an unintended type (e.g., Alphanumeric string instead of a decimal)

## 422 Unprocessable Entity

If you attempt to create an order or refund transaction that already exists in TaxJar, you'll receive a 422 error. As a fallback, make a PUT request instead and [update the existing transaction](#put-update-an-order-transaction).

Note that when creating a refund transaction, the `transaction_id` must be a unique identifier for the refund and different from the original order `transaction_id`. The `transaction_reference_id` is used to reference the original order transaction.
