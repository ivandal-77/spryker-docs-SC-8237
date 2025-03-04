---
title: Eco - Punchout Catalogs feature integration
description: Integrate Eco- Punchout Catalogs Feature into the Spryker Commerce OS.
last_updated: Jun 16, 2021
template: concept-topic-template
originalLink: https://documentation.spryker.com/2021080/docs/eco-punchout-catalogs-feature-integration
originalArticleId: 7ed75484-5360-4781-9f0f-5b0fb9a5f840
redirect_from:
  - /2021080/docs/eco-punchout-catalogs-feature-integration
  - /2021080/docs/en/eco-punchout-catalogs-feature-integration
  - /docs/eco-punchout-catalogs-feature-integration
  - /docs/en/eco-punchout-catalogs-feature-integration
  - /docs/scos/user/technology-partners/202204.0/order-management-erpoms/punchout-catalogs/eco-punchout-catalogs-feature-integration.html
  - /docs/scos/dev/technology-partner-guides/202200.0/order-management-erpoms/punchout-catalogs/eco-punchout-catalogs-feature-integration.html
---

## Install Feature Core

### Prerequisites

To start feature integration, overview and install the necessary features:

| NAME | VERSION |
| --- | --- |
| Spryker Core | 202009.0 |
| Company Account | 202009.0 |
| Cart | 202009.0 |
To start feature integration, overview and install the necessary packages:

| NAME | VERSION |
| --- | --- |
| [Punchout Catalogs Spryker](https://github.com/punchout-catalogs/punchout-catalog-spryker/) | 2.0.1 |

### 1) Install the Required Modules Using Composer

Run the following command(s) to install the required modules:

```bash
composer require spryker-eco/punchout-catalogs: "^1.0.0" --update-with-dependencies
```

{% info_block warningBox "Verification" %}

Make sure that the following modules were installed:

| MODULE | EXPECTED DIRECTORY |
|---|---|
| `PunchoutCatalogs` | `vendor/spryker-eco/punchout-catalogs` |

{% endinfo_block %}

### 2) Set up Configuration

Add the following configuration to your project:

| CONFIGURATION | SPECIFICATION | NAMESPACE |
| --- | --- | --- |
| Adjust `codeception.yml` | Enables tests execution for eco modules. | None |

**codeception.yml**

```html
include:
    - vendor/spryker-eco/*/*
```

{% info_block warningBox "Verification" %}

Make sure that when you run `codeception run`  command it also runs PunchoutCatalogs module tests.

{% endinfo_block %}

### 3) Set up Behavior

#### Set up Punchout GUI Workflow

Enable the following behaviors by registering the plugins:

| PLUGIN | SPECIFICATION | PREREQUISITES | NAMESPACE |
| --- | --- | --- | --- |
| `OciPunchoutCatalogConnectionFormatPlugin` | Expands punchout catalog connection form `Connection Format` choice field with `oci` option. | None |`SprykerEco\Zed\PunchoutCatalogs\Communication\Plugin\PunchoutCatalogs` |
| `CXmlPunchoutCatalogConnectionFormatPlugin` | Expands punchout catalog connection form `Connection Format` choice field with `xml` option. | None |`SprykerEco\Zed\PunchoutCatalogs\Communication\Plugin\PunchoutCatalogs` |
| `SetupRequestPunchoutCatalogConnectionTypePlugin` | Expands punchout catalog connection form `Connection Type` choice field with `setup_request` option. | None |`SprykerEco\Zed\PunchoutCatalogs\Communication\Plugin\PunchoutCatalogs` |

**src/Pyz/Zed/PunchoutCatalogs/PunchoutCatalogsDependencyProvider.php**

```php
<?php

namespace Pyz\Zed\PunchoutCatalogs;

use SprykerEco\Zed\PunchoutCatalogs\Communication\Plugin\PunchoutCatalogs\CXmlPunchoutCatalogConnectionFormatPlugin;
use SprykerEco\Zed\PunchoutCatalogs\Communication\Plugin\PunchoutCatalogs\OciPunchoutCatalogConnectionFormatPlugin;
use SprykerEco\Zed\PunchoutCatalogs\Communication\Plugin\PunchoutCatalogs\SetupRequestPunchoutCatalogConnectionTypePlugin;
use SprykerEco\Zed\PunchoutCatalogs\PunchoutCatalogsDependencyProvider as SprykerPunchoutCatalogsDependencyProvider;

class PunchoutCatalogsDependencyProvider extends SprykerPunchoutCatalogsDependencyProvider
{
    /**
     * @return \SprykerEco\Zed\PunchoutCatalogs\Dependency\Plugin\PunchoutCatalogConnectionFormatPluginInterface[]
     */
    protected function getPunchoutCatalogConnectionFormatPlugins(): array
    {
        return [
            new OciPunchoutCatalogConnectionFormatPlugin(),
            new CXmlPunchoutCatalogConnectionFormatPlugin(),
        ];
    }

    /**
     * @return \SprykerEco\Zed\PunchoutCatalogs\Dependency\Plugin\PunchoutCatalogConnectionTypePluginInterface[]
     */
    protected function getPunchoutCatalogConnectionTypePlugins(): array
    {
        return [
            new SetupRequestPunchoutCatalogConnectionTypePlugin(),
        ];
    }
}
```

{% info_block infoBox "Verification" %}

Make sure that on create punchout catalog connection page you can see `Connection Format` choice field with following options [oci,cxml].<br>Make sure that on create punchout catalog connection page you can see `Connection Type` choice field with following options [setup_request].

{% endinfo_block %}

Run the following command to enable Javascript and CSS changes:

```bash
console frontend:zed:build
```

{% info_block infoBox "Verification" %}

Make sure that on a connection creation page, when you change the format from `cxml` to `oci` you can see "Username" field appeared instead of "Sender ID".

{% endinfo_block %}
