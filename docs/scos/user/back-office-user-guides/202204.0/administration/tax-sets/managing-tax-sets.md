---
title: Managing tax sets
last_updated: Aug 9, 2021
template: back-office-user-guide-template
originalLink: https://documentation.spryker.com/2021080/docs/managing-tax-sets
originalArticleId: b8a16bed-2e8b-4a25-b092-d41004fe0119
redirect_from:
  - /2021080/docs/managing-tax-sets
  - /2021080/docs/en/managing-tax-sets
  - /docs/managing-tax-sets
  - /docs/en/managing-tax-sets
  - /docs/scos/user/back-office-user-guides/202200.0/administration/tax-sets/managing-tax-sets.html
---

This document describes how to create, edit, and delete [tax sets](/docs/scos/user/features/{{page.version}}/tax-feature-overview.html).

## Prerequisites

Before you can create tax sets, make sure you have [tax rates](/docs/scos/user/back-office-user-guides/{{page.version}}/administration/tax-rates/managing-tax-rates.html).  

To start working with tax sets, go to **Administration&nbsp;<span aria-label="and then">></span> Tax Sets**.

Some sections contain reference information. Make sure to review it before you start, or look up the necessary information as you go through the process.

## Creating tax sets

To use this taxes for products, threshold, or shipment methods, create a tax set and assign the tax rate to it.

To create a tax set, do the following:
1. On the **Overview of Tax Sets** page, in the top right corner of the page, click **Create Tax Set**.
2. On the **Create Tax Set** page, enter the tax set name and select one or several tax rates to assign. See [Reference information: Creating and editing tax sets](#reference-information-creating-and-editing-tax-sets).
3. Click **Save**.

![image](https://spryker.s3.eu-central-1.amazonaws.com/docs/User+Guides/Back+Office+User+Guides/Taxes/Managing+Tax+Rates/create-tax-set.png)


## Editing a tax set

To change the tax set, do the following:
1. In the **List of tax sets&nbsp;<span aria-label="and then">></span> Actions** column, click **Edit** for a specific tax set.
2. On the **Edit Tax Set** page, change the attributes.
3. Click **Save**.

**Tips and tricks**
<br>You can start editing a tax set while viewing it:
1. On the **View Tax Rate** page, in the top right corner, click **Edit**.
2. Go over the *[Editing a tax set](#editing-a-tax-set)* procedure.

### Reference information: Creating and editing tax sets

The following table describes the attributes that are used when creating or updating a tax set.

| ATTRIBUTE |DESCRIPTION  |
| --- | --- |
| Name | Name of your tax set—for example, Shipment Taxes. |
| Tax rates | List of tax rates available in the **Taxes&nbsp;<span aria-label="and then">></span> Tax Rates** section. You can assign from one to many values by selecting the checkboxes next. |

## Viewing a tax set

You can review a specific Tax Set details by clicking **View** in the **List of tax sets&nbsp;<span aria-label="and then">></span> Actions** column.
The **Tax set details** page includes the following information:
* Name
* Tax set ID
* Tax rates
* Created at
* Updated at

To return back to the list, click **Back to Tax Sets**.

## Deleting a tax set

To delete a tax set, do the following:

1. In the **List of tax sets&nbsp;<span aria-label="and then">></span> Actions** column, click **Delete** for a specific tax set.
2. To confirm the action, on the **Delete Tax Set** page, click **Delete Tax Set**.
The tax set is deleted.

{% info_block warningBox "Note" %}

Tax rates assigned to a deleted tax set become unassigned but not deleted.

{% endinfo_block %}

**Tips and tricks**
<br>You can delete a tax set while viewing it:
1. On the **View Tax Set** page, in the top right corner, click **Delete**.
2. On the **Delete Tax Set** page, confirm your action.
