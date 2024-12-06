# Hyphen Toggle OpenFeature Provider (PHP)

The **Hyphen Toggle OpenFeature Provider** is an OpenFeature provider implementation for the Hyphen Toggle platform. It enables feature flag evaluation using the OpenFeature standard in PHP.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Usage](#usage)
3. [Configuration](#configuration)
4. [Contributing](#contributing)
5. [License](#license)

---

## Getting Started

### Installation

Install the provider and the OpenFeature SDK for PHP:

```bash
composer require open-feature/sdk hyphen/openfeature-server-provider
```

## Usage

### Example: Basic Setup

To integrate the Hyphen Toggle provider into your application, follow these steps:

1. **Set up the provider**: Register the `HyphenProvider` with OpenFeature using your `publicKey` and provider options.
2. **Evaluate a feature toggle**: Use the client to evaluate a feature flag.

```php
use OpenFeature\OpenFeatureAPI;
use Hyphen\OpenFeature\HyphenProvider;

require 'vendor/autoload.php';

$publicKey = "your-public-key-here";

$options = [
'application' => 'your-application-name',
'environment' => 'production',
];

$provider = new HyphenProvider($publicKey, $options);

$openFeatureAPI = OpenFeatureAPI::getInstance();
$openFeatureAPI->setProvider($provider);

$client = $openFeatureAPI->getClient();

$flagDetails = $client->getBooleanDetails('feature-flag-key', false);

echo $flagDetails->getValue() ? 'true' : 'false';
```

### Example: Contextual Evaluation

To evaluate a feature flag with specific user or application context, define and pass an `EvaluationContext`:

```php
use OpenFeature\OpenFeatureAPI;
use Hyphen\OpenFeature\HyphenProvider;
use OpenFeature\EvaluationContext;

$context = new EvaluationContext([
  'targetingKey' => 'user-123',
  'ipAddress' => '203.0.113.42',
  'customAttributes' => [
    'subscriptionLevel' => 'premium',
    'region' => 'us-east',
  ],
  'user' => [
    'id' => 'user-123',
    'email' => 'user@example.com',
    'name' => 'John Doe',
    'customAttributes' => [
      'role' => 'admin',
    ],
  ],
]);

$flagDetailsWithContext = $client->getBooleanDetails('feature-flag-key', false, $context);

echo $flagDetailsWithContext->getValue() ? 'true' : 'false';
```

## Configuration

### Options

| Option          | Type   | Description                                                                           |
|------------------|--------|---------------------------------------------------------------------------------------|
| `application`    | string | The application id or alternate id.                                                  |
| `environment`    | string | The environment in which your application is running (e.g., `production`, `staging`). |

### Context

Provide an `EvaluationContext` to pass contextual data for feature evaluation.

### Context Fields

| Field               | Type                  | Description                                                                 |
|---------------------|-----------------------|-----------------------------------------------------------------------------|
| `targetingKey`      | `string`             | The key used for caching the evaluation response.                          |
| `ipAddress`         | `string`             | The IP address of the user making the request.                             |
| `customAttributes`  | `array`              | Custom attributes for additional contextual information.                   |
| `user`              | `array`              | An array containing user-specific information for the evaluation.          |
| `user.id`           | `string`             | The unique identifier of the user.                                         |
| `user.email`        | `string`             | The email address of the user.                                             |
| `user.name`         | `string`             | The name of the user.                                                      |
| `user.customAttributes` | `array`          | Custom attributes specific to the user.                                    |

## Contributing

We welcome contributions to this project! If you'd like to contribute, please follow the guidelines outlined in [CONTRIBUTING.md](CONTRIBUTING.md). Whether it's reporting issues, suggesting new features, or submitting pull requests, your help is greatly appreciated!

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for full details.