# Lunu Payment Salesforce Commerce Cloud Integration

## About

Lunu is a cryptocurrency payment gateway that enables seamless crypto payments in your Salesforce Commerce Cloud storefront. The integration supports any software wallet that recognizes cryptocurrency addresses and transaction amounts, using industry-standard formats:
- **EIP-67** format for Ethereum QR codes
- **BIP-21** format for Bitcoin QR codes

This cartridge provides a complete payment integration including payment processing, webhook handling, and automated order management.

## Features

- ✅ Cryptocurrency payment processing (Bitcoin, Ethereum, and more)
- ✅ Real-time payment notifications via webhooks
- ✅ Automated order status updates
- ✅ Business Manager job for processing payment notifications
- ✅ SFRA (Storefront Reference Architecture) compatible
- ✅ Secure API communication with authentication
- ✅ Customizable payment widget URL
- ✅ Test and production mode support
- ✅ Comprehensive logging for troubleshooting

## Requirements

- Salesforce Commerce Cloud (SFCC) instance
- SFRA-compatible storefront (version 5.0+)
- Node.js 4.0 or higher (for development)
- Active Lunu merchant account with API credentials

## Installation

### 1. Upload Cartridges

Upload the following cartridges to your Salesforce Commerce Cloud instance:

- `int_lunupayment` - Core integration cartridge
- `int_lunupayment_sfra` - SFRA-specific integration
- `bm_lunupayment` - Business Manager extensions

### 2. Configure Cartridge Path

Add the cartridges to your site's cartridge path in Business Manager:

**Administration > Sites > Manage Sites > [Your Site] > Settings**

Add to the beginning of your cartridge path:
```
int_lunupayment_sfra:int_lunupayment:bm_lunupayment
```

Example complete path:
```
int_lunupayment_sfra:int_lunupayment:bm_lunupayment:app_storefront_base
```

### 3. Import Metadata

Import the metadata files from the `metadata` directory:

1. **Site Import**: Go to **Administration > Site Development > Site Import & Export**
2. Upload and import the following in order:
   - `metadata/meta/custom-objecttype-definitions.xml` (Custom Objects)
   - `metadata/meta/system-objecttype-extensions.xml` (Site Preferences)
   - `metadata/services.xml` (Service Configuration)
   - `metadata/sites/RefArch/payment-methods.xml` (Payment Methods)
   - `metadata/sites/RefArch/payment-processors.xml` (Payment Processors)
   - `metadata/jobs.xml` (Job Configuration)

3. **Import Static Assets**: Import the payment icon from `metadata/libraries/`

### 4. Configure Services

Configure the Lunu API service credentials:

**Administration > Operations > Services > Credentials**

Find `http.lunupayment.cred` and configure:
- **URL**: `https://alpha.lunu.io/api/v1/` (or production URL)
- Leave User ID and Password empty (authentication uses custom headers)

### 5. Configure Site Preferences

**Merchant Tools > Site Preferences > Custom Preferences > Lunu Payment**

Configure the following settings:

| Setting | Description | Example |
|---------|-------------|---------|
| **Lunu Enabled** | Enable/disable the integration | `true` |
| **Lunu App ID** | Your Lunu application ID | `your-app-id` |
| **Lunu Secret Key** | Your Lunu secret key | `your-secret-key` |
| **Lunu Widget URL** | Payment widget URL template | `https://widget.lunupay.com/sandbox/#/?action=select&cancel={cancelURL}&success={successURL}&token={token}` |

**Security Note**: Keep your Secret Key confidential and never commit it to version control.

### 6. Configure Payment Method

**Merchant Tools > Ordering > Payment Methods**

1. Find the **LUNU** payment method
2. Enable it for your site
3. Configure applicable countries and currencies

### 7. Set Up Notification Processing Job

**Administration > Operations > Jobs**

1. Find the `LunuProcessNotifications` job
2. Configure the schedule:
   - Recommended: Run every 5 minutes
   - Enable the job
   - Set appropriate time zone

This job processes incoming payment notifications and updates order statuses automatically.

## Configuration

### Widget URL Placeholders

The Widget URL supports the following placeholders:

- `{token}` - Lunu payment confirmation token
- `{successURL}` - Callback URL for successful payments
- `{cancelURL}` - Callback URL for canceled/failed payments

### Environment Configuration

For **Production** environments:
- Update the service credential URL to production endpoint
- Use production Lunu App ID and Secret Key
- Update Widget URL to production widget

For **Development/Staging**:
- Use sandbox endpoints
- Use test credentials
- Test mode flag will be set automatically

## Usage

### Checkout Flow

1. Customer adds products to cart
2. Proceeds to checkout
3. Selects **Lunu** as payment method
4. Completes order placement
5. Redirected to Lunu payment widget
6. Completes crypto payment
7. Automatically redirected back to confirmation page

### Order Management

Orders are automatically updated based on payment status:

- **Paid**: Order status set to PAID, ready for fulfillment
- **Failed**: Order automatically failed, basket restored
- **Expired**: Order failed, customer can retry
- **Canceled**: Order canceled, basket restored

### Webhooks

The integration automatically handles webhooks from Lunu at:
```
https://your-site.com/on/demandware.store/Sites-SiteID-Site/default/LunuPayment-ChangeStatus
```

Webhook notifications are stored as Custom Objects and processed by the scheduled job.

## Development

### Build Assets

To compile JavaScript assets for the SFRA cartridge:

```bash
npm install
npm run build
```

### Linting

To check code quality:

```bash
npm run lint
```

### Testing

Run integration tests:

```bash
npm run test:integration
```

**Note**: Update `test/integration/it.config.js` with your sandbox URL before running tests.

## Troubleshooting

### Common Issues

**Payment not processing**
- Check that the `LunuProcessNotifications` job is running
- Verify webhook URL is accessible from Lunu servers
- Check Business Manager logs for errors

**Service errors**
- Verify API credentials are correct
- Check service configuration URL
- Review communication logs in Business Manager

**Order not updating**
- Check Custom Object `LunuPaymentNotification` for pending notifications
- Verify job execution logs
- Check order status in Business Manager

### Logging

Logs are available in Business Manager:

**Administration > Operations > Custom Log Files**

Look for:
- `Lunu` - General Lunu payment logs
- `LunuLogger` - Controller and service logs

### Debug Mode

Enable service communication logging:

**Administration > Operations > Services > Services**

Find `http.lunupayment` and enable:
- Communication Log

## API Documentation

For detailed API documentation, refer to:
- [Lunu API Documentation](https://docs.lunupay.com/api)
- [Integration Guide](documentation/LUNU.pdf)

## Support

- **Documentation**: [documentation/LUNU.pdf](documentation/LUNU.pdf)
- **Issues**: Report issues in the project repository
- **Lunu Support**: Contact your Lunu account manager

## Version History

See [CHANGELOG.md](CHANGELOG.md) for version history and updates.

## License

This project is licensed under the ISC License - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please ensure:
- Code follows existing style (ESLint configuration)
- All tests pass
- Documentation is updated
- Commit messages are descriptive

## Security

If you discover a security vulnerability, please email security contact rather than using the issue tracker.

**Never commit sensitive data**:
- API credentials
- Secret keys
- Production URLs
- Customer data

## Cartridge Structure

```
├── cartridges/
│   ├── bm_lunupayment/           # Business Manager integration
│   │   └── cartridge/
│   │       └── scripts/
│   │           └── steps/         # Job step implementations
│   ├── int_lunupayment/          # Core integration
│   │   └── cartridge/
│   │       ├── controllers/       # Payment controllers
│   │       ├── scripts/
│   │       │   ├── helpers/       # Helper functions
│   │       │   ├── hooks/         # Payment hooks
│   │       │   └── services/      # API service definitions
│   │       └── templates/         # ISML templates
│   └── int_lunupayment_sfra/     # SFRA-specific code
│       └── cartridge/
│           ├── client/            # Frontend JavaScript
│           ├── controllers/       # SFRA controller extensions
│           └── templates/         # SFRA templates
├── metadata/                      # Import/export metadata
│   ├── jobs.xml                   # Job definitions
│   ├── services.xml               # Service configuration
│   ├── meta/                      # Object definitions
│   └── sites/                     # Site-specific config
└── test/                          # Integration tests
```

## Credits

Built for Salesforce Commerce Cloud using the Storefront Reference Architecture (SFRA).
