---
title: Exporting data
description: This article will teach you how to export data from a Spryker shop to an external system
last_updated: Jun 16, 2021
template: data-export-template
originalLink: https://documentation.spryker.com/2021080/docs/exporting-data
originalArticleId: 0a38b991-f10c-4f6c-90db-247a62cda2e7
redirect_from:
  - /2021080/docs/exporting-data
  - /2021080/docs/en/exporting-data
  - /docs/exporting-data
  - /docs/en/exporting-data
related:
  - title: Sales Data Export feature integration
    link: docs/scos/dev/feature-integration-guides/page.version/sales-data-export-feature-integration.html
  - title: Data Export Orders .csv Files Format
    link: docs/scos/dev/data-export/page.version/data-export-orders-.csv-files-format.html
---

To quickly populate an external system like ERP or OMS with data from your Spryker shop, you can export it as CSV files from the Spryker shop and then import them into the external system.

For now, you can export only order data, which includes data on:
* Orders
* Order items
* Order expenses

{% info_block infoBox "Export file format" %}

Currently, we only support CSV as a format for file exports out of the box.

{% endinfo_block %}

To export the order data, you need to:

1. Make sure you have the[ Sales Data Export feature installed](/docs/scos/dev/feature-integration-guides/{{page.version}}/sales-data-export-feature-integration.html) for your project.
2. Specify necessary configurations in the YML export configuration file residing in `./data/export/config/`. For details about the YML export configuration file structure and configuration options, see [.yml Export Configuration File](#yml-export-configuration-file).
3. Run `console data:export --config file-name.yml`, where `file-name.yml` is the name of the YML export configuration file. The command creates export CSV files in the `./data/export/`folder for each `data_entity` of the YML file. For each store specified in the YML file, a separate file is created. For an example of how the export works, see [Structure of the YML export configuration file](/docs/scos/dev/data-export/{{page.version}}/data-export.html#structure).

{% info_block infoBox "Multi-store support" %}

Spryker Data Export supports the multi-store functionality, which means that you can export data for multiple stores.

{% endinfo_block %}

## YML export configuration file

The YML export configuration file lets you define what orders you want to export. The following content is exported:
* order
* order-item
* order-expense

By default, the YML export configuration file resides in `./data/export/config/`. You can adjust your YML export configuration file, but when doing so, stick to its [structure](/docs/scos/dev/data-export/{{page.version}}/data-export.html#structure) and take the possible [data filtering options](/docs/scos/dev/data-export/{{page.version}}/data-export.html#filter) into account.

{% info_block warningBox "Note" %}

The root of data export files is configured globally, and it is not changeable by data export.

{% endinfo_block %}

<a name="structure"></a>

### Structure of the YML export configuration file

Structure of the YML export configuration file is as follows:

```yml
defaults:
  filter_criteria: &default_filter_criteria
        order_created_at:  
            type: between
            from: '<order_created_at_date-time_from_value>'
            to: '<order_created_at_date-time_to_value>'

actions:  
   - data_entity: order
      destination: '{data_entity}s_<store_name_value_1>_{timestamp}.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: [<store_name_value_1>]
    - data_entity: order
      destination: '{data_entity}s_<store_name_value_2>_{timestamp}.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: [<store_name_value_2>]
    - data_entity: order
      destination: '{data_entity}s_<store_name_value_n>_{timestamp}.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: [<store_name_value_n>]

   - data_entity: order-item
      destination: '{data_entity}s_<store_name_value_1>_{timestamp}.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: [<store_name_value_1>]

   -  data_entity: order-expense             
      destination: '{data_entity}s_<store_name_value_1>_{timestamp}.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: [<store_name_value_1>]
```
Type of content to export is defined in section `actions` by `data_entity` and must be `order`, `order-item` and `order-expense` . You can define what stores you want to run export for, and specify order dates you want to export data for. For the infomration about how to export order data for specific stores and time period, see [Setting the Filter Criteria](/docs/scos/dev/data-export/{{page.version}}/data-export.html#filter) in a YML file.

For example, check out the default YML export configuration file [order_export_config.yml](https://github.com/spryker-shop/suite/blob/master/data/export/production/order_export_config.yml). It’s configuration presupposes batch export of the three data entities: `order`, `order-item`, `order-expense.`

When running the command for data export with this file, `console data:export --config order_export_config.yml`, exported CSV files are created in `data/export`. For each data entity and store, a separate file is generated, namely:

* order-expenses_AT.csv
* order-expenses_DE.csv
* order-items_AT.csv
* order-items_DE.csv
* orders_AT.csv
* orders_DE.csv

For details about the content of each of the files, see[ Data Export Ordres CSV Files Format](/docs/scos/dev/data-export/{{page.version}}/data-export-orders-.csv-files-format.html).
<a name="filter"></a>

### Setting the filter criteria in a YML file

You can set the following filter criteria for the order data export in your YML export configuration file:

* *Store names*: stores from which the data are exported.
* *Date and time range*: interval *from* what date and time *to* what date and time the order was created, including the *from* and the *to* values. If you use the label `order_updated_at`, the range is relative to the date and time the order was updated.

#### Defining the stores for order data export

To define the stores you want to export the order data for, specify them in `destination` for the specific data entities. Keep in mind that you have to create individual files for each data entity and for each store if your filter criteria include `store_name`.

For example, if you want to export the `order-expenses` data for the DE store, and `order-items` data for DE and AT stores, your YML file should look like this:

```yml
defaults:
    filter_criteria: &default_filter_criteria
        order_created_at:
            type: between
            from: '2020-05-01 00:00:00'
            to: '2020-12-31 23:59:59'

actions:
    - data_entity: order-expense
      destination: '{data_entity}s_DE_{timestamp}.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: [DE]

    - data_entity: order-item
      destination: '{data_entity}s_DE_{timestamp}.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: [DE]
    - data_entity: order-item
      destination: '{data_entity}s_AT_{timestamp}.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: [AT]
  ```
#### Defining the Date and Time Range for Order Data Export

The default date and time range filter criteria, for example, the order creation dates filter applied to all `data_entity` items by default, is specified in the `defaults` section:

```yml
defaults:
  filter_criteria: &default_filter_criteria
        order_created_at:  
            type: between
            from: '2020-05-01 08:00:00'
            to: '2020-06-07 08:00:00'  
 ```

{% info_block infoBox "Info" %}

To use the the date and time range filter criteria of the `defaults` section, and apply the filter criteria to `data_entity` items of the `actions` section, `<<: *default_filter_criteria` must be declared in the `filter_criteria` parameter of each `data_entity` item.

{% endinfo_block %}

You can change the filter criteria for any `data_entity` items by replacing `<<: *default_filter_criteria` with the values you need.

For example, suppose that for the `order-expenses` data entity in the AT store you want to export only orders that were created on May 15th, 2020. In this case, you replace the `<<: *default_filter_criteria` line under `filter_criteria` for `order-expenses` in AT store with this:

```yml
order_created_at:  
  type: between
  from: '2020-05-15 00:00:00'
  to: '2020-05-15 23:59:59'
  ```
  Example:

  ```yml
  defaults:
  filter_criteria: &default_filter_criteria
        order_created_at:  
            type: between
            from: '2020-05-01 00:00:00'
            to: '2020-06-07 23:59:59'

actions:  
   - data_entity: order-expense
      destination: '{data_entity}s_DE.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: DE
    - data_entity: order-expense
      destination: '{data_entity}s_AT.{extension}'
      filter_criteria:
          order_created_at:  
            type: between
            from: '2020-05-15 00:00:00'
            to: '2020-05-15 23:59:59'
          store_name: AT
    ...
```

After running the command with the changed filter criteria for `order-expense`, the `order-expenses_AT.csv` file will only contain the orders created on May 15th, 2020. The other files will contain the orders created from May 1st till July 6th, as specified in `&default_filter_criteria` of the `defaults` section.

## Overwriting existing CSV files upon repeated command run

When exporting data, the newly generated CSV files overwrite the existing ones. Currently, this behavior is not configurable.

If you want to generate new CSV files without overwriting eventual existing ones, you may use a `{timestamp}` tag in the name of the file to be generated. For example, if you use the default structure of the yaml export configuration file, upon repeated launch of the `console data:export --config file-name.yml`, the already existing export csv files will be generated with different file names according to the `{timestamp}` on the moment of its creation, and therefore will not be overwritten.
And vice versa: if you want to overwrite the existing files, remove `{timestamp}` from the `destination` parameter of the YML file for the necessary `data_entity` items, for example:

Initial file:
```yml
defaults:
  filter_criteria: &default_filter_criteria
        order_created_at:  
            type: between
            from: '2020-05-01 08:00:00'
            to: '2020-06-07 08:00:00'

actions:  
   - data_entity: order-expense
      destination: '{data_entity}s_DE_{timestamp}.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: DE
    - data_entity: order-expense
      destination: '{data_entity}s_AT_{timestamp}.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: AT
    ...
```

File with the removed `{timestamp}`:
```yml
defaults:
  filter_criteria: &default_filter_criteria
        order_created_at:  
            type: between
            from: '2020-05-01 08:00:00'
            to: '2020-06-07 08:00:00'

actions:  
   - data_entity: order-expense
      destination: '{data_entity}s_DE.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: DE
    - data_entity: order-expense
      destination: '{data_entity}s_AT.{extension}'
      filter_criteria:
          <<: *default_filter_criteria
          store_name: AT
    ...
 ```
