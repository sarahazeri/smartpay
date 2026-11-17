# SmartPay

A PHP SDK for the SmartPay payment gateway. It builds and encrypts payment requests and decrypts the gateway response, so you do not have to handle the encryption details yourself.

For the gateway's own integration documentation, please refer to the official SmartPay merchant integration guide provided by your bank or SmartPay account manager.

## Usage

```php
require 'vendor/autoload.php';

use SmartPay\SmartPay;

$smartPay = new SmartPay('YOUR_MERCHANT_ID', 'YOUR_ACCESS_CODE', 'YOUR_WORKING_KEY');

// Start a payment
$response = $smartPay->initiatePayment(
    'ORDER12345',
    '10.50',
    'OMR',
    'https://yourwebsite.com/success',
    'https://yourwebsite.com/cancel'
);
echo $response;

// Process the encrypted response sent back by the gateway
$encResponse = 'RECEIVED_ENCRYPTED_RESPONSE';
$decryptedResponse = $smartPay->processResponse($encResponse);
echo $decryptedResponse;
```

## Transactions table

You need a table to store transactions. You can create it with your own structure, or use the migration included in this package:

```php
require 'vendor/autoload.php';

use SmartPay\Database\Migrations\CreateTransactionsTable;

CreateTransactionsTable::up();
```

## Security notes

- Never commit your merchant ID, access code, or working key. Load them from environment variables or a secrets manager.
- Always use HTTPS for the success and cancel URLs and for all communication with the gateway.
- Validate and sanitise data before sending a request and after receiving a response.
