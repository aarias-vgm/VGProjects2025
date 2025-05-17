# Consignación

```mermaid
---
title: CONSIGNMENT AND REPLENISHMENT ORDERS
---
%%{
  init: {
    "flowchart": {
      "htmlLabels": true,
      "curve": "basis"
    }
  }
}%%
flowchart TB
   classDef invisible display:none;
   classDef invisible height:0px;
   classDef invisibleXL display:none;
   classDef invisibleXL height:25px;
   classDef noWrap white-space:nowrap;
   classDef leftAligned text-align:left;

   abc@{ shape: brace-r, label: "PO: Purchase Order<br/>CO: Consignment Order<br/>RO: Replenishment Order" }

   subgraph reception ["Reception of products"]
      direction TB

      coExample("**➡️ Purchase Order**<br/><code>[Consignment]</code><br/>")

      subgraph coStockIn ["Inventory WH/IN ⬇️<br/>owner = vendor (readonly)"]
         direction LR
         
         dummy1[" "]:::invisible
         receptionProvider>"🏢 Partners/Vendors"]
         receptionStock>"📦 WH/Stock"]

         receptionProvider -- "Send to ⛟" --> receptionStock
      end
      
      receptionJournalIn("`**Journal entry WH/IN ✏️**<br/>_0% taxes_<br/><table style="border-collapse: collapse;"><tr style="border-bottom: 1px solid darkgray;"><th style="padding: 8px 16px;"></th><th style="padding: 8px 16px;">Debid</th><th style="padding: 8px 16px" ;>Credit</th></tr><tr style="border-bottom: 1px solid dimgray;"><td style="padding: 4px;">Stock valuation account</td><td style="padding: 4px;">✔️</td><td style="padding: 4px;"></td></tr><tr style="border-bottom: 1px solid dimgray;"><td style="padding: 4px;">Stock input account</td><td style="padding: 4px;"></td><td style="padding: 4px;">✔️</td></tr></table>`"):::noWrap
      invoiceLocked["`**🔒 Permanent invoice blocking**<br/>Don't allow CO to create invoices ⚠️`"]:::noWrap
      
      coExample --> coStockIn --> receptionJournalIn --> invoiceLocked
   end

   subgraph saleOrder ["Sale Order \(SO\)"]
      saleOrderProducts@{ shape: comment, label: "<small>Sale Order may contain products that are either consigned or not</small>"}
   end

   isExclusive{"Main contact<br/>of delivery<br/>address = customer"}

   soExclusiveUse("`**🏷️ Sale Order**<br/><code>[Quotation]</code><br/><small>(exclusive use)</small><br/><table><tr><td style="text-align: left;">Customer: HeartWell</td></tr><tr><td style="text-align: left;">Delivery Address: HeartWell, North Branch</td></tr></table><table style="border-collapse: collapse; white-space: nowrap;"><tr style="border-bottom: 1px solid darkgray;"><th style="padding: 8px 8px;"></th><th style="padding: 8px 8px;">Units</th><th style="padding: 8px 8px;">Product</th><th style="padding: 8px 8px;">Owner</th><th style="padding: 8px 8px;">Warehouse</th><th style="padding: 8px 8px; text-align: left;">Route</th></tr><tr style="border-bottom: 1px solid dimgray;"><td style="padding: 4px 8px;">1</td><td style="padding: 4px 8px;">5</td><td style="padding: 4px 8px;">Virexa</td><td style="padding: 4px 8px;"></td><td style="padding: 4px 8px;">WH Bogotá</td><td style="padding: 4px 8px; text-align: left;">WH Bogotá: Pickup</td></tr><tr style="border-bottom: 1px solid dimgray;"><td style="padding: 4px 8px;">2</td><td style="padding: 4px 8px;">1</td><td style="padding: 4px 8px;">CoreFix Align</td><td style="padding: 4px 8px;"></td><td style="padding: 4px 8px;">Event</td><td style="padding: 4px 8px; text-align: left;">Event: [HeartWell, North Branch] Pickup</td></tr><tr style="border-bottom: 1px solid dimgray;"><td style="padding: 4px 8px;">3</td><td style="padding: 4px 8px;">2</td><td style="padding: 4px 8px;">Neuravax</td><td style="padding: 4px 8px;">Neuravia</td><td style="padding: 4px 8px;">WH Bogotá</td><td style="padding: 4px 8px; text-align: left;">WH Bogotá: Pickup</td></tr><tr style="border-bottom: 1px solid dimgray;"><td style="padding: 4px 8px;">4</td><td style="padding: 4px 8px;">10</td><td style="padding: 4px 8px;">Surginex</td><td style="padding: 4px 8px;"></td><td style="padding: 4px 8px;">Consignment</td><td style="padding: 4px 8px; text-align: left;">Consignment: [HeartWell, North Branch] Pickup (exclusive use)</td></tr></table>`"):::noWrap
      
   soLinesCases@{ shape: braces, label: "Lines:<br/>1) No consignment<br/>2) No consignment (event)<br/>3) Me as consignee<br/>4) Me as consignor" }
   
   soSharedUse("`**🏷️ Sale Order**<br/><code>[Quotation]</code><br/><small>(shared use)</small><br/><table><tr><th style="text-align: left;">Customer: OxinovaLab</th></tr><tr><th style="text-align: left;">Delivery Address: HeartWell, North Branch</th></tr></table><table style="border-collapse: collapse; white-space: nowrap;"><tr style="border-bottom: 1px solid darkgray;"><th style="padding: 8px 8px;"></th><th style="padding: 8px 8px;">Units</th><th style="padding: 8px 8px;">Product</th><th style="padding: 8px 8px;">Owner</th><th style="padding: 8px 8px;">Warehouse</th><th style="padding: 8px 8px; text-align: left;">Route</th></tr><tr style="border-bottom: 1px solid dimgray;"><td style="padding: 4px 8px;">1</td><td style="padding: 4px 8px;">5</td><td style="padding: 4px 8px;">Virexa</td><td style="padding: 4px 8px;"></td><td style="padding: 4px 8px;">WH Bogotá</td><td style="padding: 4px 8px; text-align: left;">WH Bogotá: Pickup</td></tr><tr style="border-bottom: 1px solid dimgray;"><td style="padding: 4px 8px;">2</td><td style="padding: 4px 8px;">1</td><td style="padding: 4px 8px;">CoreFix Align</td><td style="padding: 4px 8px;"></td><td style="padding: 4px 8px;">Event</td><td style="padding: 4px 8px; text-align: left;">Event: [Heartwell, North Branch] Pickup</td></tr><tr style="border-bottom: 1px solid dimgray;"><td style="padding: 4px 8px;">3</td><td style="padding: 4px 8px;">2</td><td style="padding: 4px 8px;">Neuravax</td><td style="padding: 4px 8px;">Neuravia</td><td style="padding: 4px 8px;">WH Bogotá</td><td style="padding: 4px 8px; text-align: left;">WH Bogotá: Pickup</td></tr><tr style="border-bottom: 1px solid dimgray;"><td style="padding: 4px 8px;">4</td><td style="padding: 4px 8px;">10</td><td style="padding: 4px 8px;">Surginex</td><td style="padding: 4px 8px;"></td><td style="padding: 4px 8px;">Consignment</td><td style="padding: 4px 8px; text-align: left;">Consignment: [HeartWell, North Branch] Pickup (shared use)</td></tr></table>`"):::noWrap

   automaticRouting@{ shape: brace-r, label: "Automatic routing adding warehouse field"}

   subgraph line1 ["Line 1: Virexa"]
      direction LR

      line1Origin>"📦 WH-BG/Stock"]
      line1Destiny>"👤 Partners/Customers"]

      line1Origin -- "Validate" --> line1Destiny
   end

   subgraph line2 ["Line 2: CoreFix Align"]
      direction LR

      subgraph line2Origin ["Example of resupply chain"]
         direction BT

         line2Event>"🏥 EVENT/Stock/<br/>HeartWell/North Branch"]:::noWrap
         line2Warehouse>"📦 WH-BG/Stock"]:::noWrap
         line2ConsignmentShared>"🏢 CONS/Stock/HeartWell/<br/>North Branch/shared"]:::noWrap
         line2Hold>"👩🏻‍🦰 HOLD/Stock/Carmen"]:::noWrap

         line2Hold -- "If not enough stock" --> line2ConsignmentShared -- "If not enough stock" --> line2Warehouse -- "If not enough stock" --> line2Event
      end
      
      line2Destiny>"📦 WH-BG/Stock"]

      line2Origin -- "Validate" --> line2Destiny
   end

   subgraph line3 ["Line 3: Neuravax"]
      direction LR

      line3Origin>"📦 WH-BG/Stock"]
      line3Destiny>"👤 Partners/Customers"]

      line3Origin -- "Validate" --> line3Destiny
   end

   subgraph line4 ["Line4: Surginex"]
      direction LR
      
      line4ConsignmentExclusiveComment@{ shape: brace-r, label: "If exclusive use❗"}
      line4ConsignmentExclusive["🏢 CONS/Stock/HeartWell/<br/>North Branch/exclusive"]:::noWrap
      
      line4ConsignmentExclusiveShared@{ shape: brace-r, label: "If shared use❗"}
      line4ConsignmentShared["🏢 CONS/Stock/HeartWell/<br/>North Branch/shared"]:::noWrap

      line4Destiny>"👤 Partners/Customers"]:::noWrap

      line4ConsignmentExclusiveComment --- line4ConsignmentExclusive -- "Validate" --> line4Destiny
      line4ConsignmentExclusiveShared --- line4ConsignmentShared -- "Validate" --> line4Destiny
   end

   afterMedicalProcedure["After medical procedure&nbsp;🩺"]

      subgraph expenseSheet ["Expense Sheet"]
         direction TB

         physicalExpenseSheet@{ shape: flag, label: "_Physical Expense Sheet_<br/><small>Details the consumed products and additional ones (if any)</small>"}
         
         virtualExpenseSheet@{ shape: div-rect, label: "_Odoo Expense Sheet_<br/><table style="border-collapse: collapse;"><tr style="border-bottom: 1px solid gray;"><td>PRODUCT</td><td>TAKEN</td><td>USED</td><td>RECEIVED</td><td>LOST</td></tr><tr style="border-bottom: 1px solid gray;"><td>Virexa</td><td>5</td><td>0</td><td>5</td><td>0</td></tr><tr style="border-bottom: 1px solid gray;"><td>Neuravax</td><td>2</td><td>2</td><td>0</td><td>0</td></tr><tr style="border-bottom: 1px solid gray;"><td>Surginex</td><td>10</td><td>2</td><td>7</td><td>1</td></tr><tr style="border-bottom: 1px solid gray;"><td>ReVita</td><td>1</td><td>1</td><td>0</td><td>0</td></tr></table>"}
         expenseSheetNote@{ shape: brace-r, label: "It can be written by someone from within the company or an outsider"}
         
         esButton((("Record<br/>returns and<br/>deliveries")))

         saleOrderChanges[["Sale Order changes"]]

         unplannedProducts["1 unit of ReVita"]
         unusedProducts["5 units of Virexa"]

         stockPickingChanges[["Stock Picking changes"]]

         subgraph esReturnStockFlow ["Inventory WH/RET ↩️"]
            direction TB

            esReturnEvent>"🏥 EVENT/Stock/HeartWell"]
            esReturnStock>"📦 WH/Stock"]
            esReturnConsignedStock>"📦 CONS/HeartWell"]

            esReturnEvent -- "Send to ⛟" --> esReturnStock
            esReturnEvent -- "Send to ⛟" --> esReturnConsignedStock
         end

         subgraph esDeliverStockFlow ["Inventory WH/OUT ⬆️"]
            direction TB

            esDeliverEvent>"🏥 EVENT/Stock/HeartWell"]
            esDeliverCustomers>"👤 Partners/Customers"]

            esDeliverEvent -- "Send to ⛟" --> esDeliverCustomers
         end

         esJournalOut("`**Journal#nbsp;entry#nbsp;WH/OUT#nbsp;✏️**<br/>_0% taxes_<br/><table style="border-collapse: collapse;"><tr style="border-bottom: 1px solid gray;"><th></th><th>Debid</th><th>Credit</th></tr><tr style="border-bottom: 1px solid gray;"><td>Stock#nbsp;output#nbsp;account</td><td>✔️</td><td></td></tr><tr style="border-bottom: 1px solid gray;"><td>Stock#nbsp;valuation#nbsp;account</td><td></td><td>✔️</td></tr></table>`")

         physicalExpenseSheet --- expenseSheetNote
         physicalExpenseSheet --> virtualExpenseSheet -- "Appears button" --> esButton

         esButton -- "automatically performs" --> stockPickingChanges
         esButton -- "automatically performs" --> saleOrderChanges

         stockPickingChanges -- "↩ Return unused products" --> esReturnStockFlow
         stockPickingChanges -- "⬆ Deliver used products" --> esDeliverStockFlow --> esJournalOut

         saleOrderChanges -- "✚ Add lines of<br/>unplanned products" --> unplannedProducts
         saleOrderChanges -- "– Delete lines of<br/>unused products" --> unusedProducts
      end

   newSaleInvoice(["🧾 Sale invoice creation"])
   
   subgraph replenishment["Replenishment"]
      direction TB

      hasOwner{"SO product on<br/>consignment?<br/>(Has owner?)"}

      deliveryPolitic@{ shape: braces, label: "\"I buy what I sell\"" }

      subgraph purchase ["Purchase"]
         direction TB

         subgraph purchaseOrder [" "]
            direction BT

            roExample("**🛒 Purchase Order**<br/><code>[Replenishment]</code><br/>_(Readonly)_<br/><table style="border-collapse: collapse;"><tr style="border-bottom: 1px solid gray;"><th>Units</th><th>Product</th><th>Vendor</th><th>Deliver#nbsp;To</th></tr><tr style="border-bottom: 1px solid gray;"><td>2</td><td>Neuravax</td><td>Neuravia</td><td>WH/Stock</td></tr></table>")
            replenishmentNote@{ shape: comment, label: "Allow<br/>invoice<br/>creation" }
         end

         subgraph roStockIn ["Inventory WH/IN ⬇️<br/>owner = vendor (readonly)"]
            direction LR
            
            dummy10[" "]:::invisible
            roProvider>"🏢 Partners/Vendors"]
            roStock>"📦 WH/Stock"]

            roProvider -- "Send to ⛟" --> roStock
         end

         roJournalIn("`**Journal#nbsp;entry#nbsp;WH/IN#nbsp;✏️**<br/>_0% taxes_<br/><table style="border-collapse: collapse;"><tr style="border-bottom: 1px solid gray;"><th></th><th>Debid</th><th>Credit</th></tr><tr style="border-bottom: 1px solid gray;"><td>Stock#nbsp;valuation#nbsp;account</td><td>✔️</td><td></td></tr><tr style="border-bottom: 1px solid gray;"><td>Stock#nbsp;input#nbsp;account</td><td></td><td>✔️</td></tr></table>`")

         newInvoice(["🧾 Purchase invoice creation"])

         roExample -- "Confirm" --> roStockIn --> roJournalIn --> newInvoice
      end

      hasOwner -- Yes --- deliveryPolitic -- "Create automatically" --> purchase
   end

   %% pcm -- "Has relation with" --- saleOrder
   abc ~~~ reception

   reception -- "Sale of products" --> saleOrder
   
   saleOrder --- isExclusive

   isExclusive -- "Yes: exclusive use consignment" --- soExclusiveUse
   isExclusive ~~~ soLinesCases
   isExclusive -- "No: shared use consignment" --- soSharedUse
   
   %% soLinesCases ~~~ soSharedUse

   soExclusiveUse & soSharedUse ~~~ automaticRouting
   automaticRouting ~~~ line1 ~~~ line2
   automaticRouting ~~~ line3 ~~~ line4
   
   line2 & line4 ~~~ afterMedicalProcedure
   
   afterMedicalProcedure --> expenseSheet
   
   expenseSheet -- "After automatic changes" --> newSaleInvoice

   newSaleInvoice --> replenishment

%% lost = taken - used - received
```

## Consignment order

### Detalles del desarrollo de orden de consignación

1. _Purchase Order (`purchase.order`) model modifications:_
   1. New field: order_type
      - Type: Selection
      - Required: `true`
      - Options:
        - Purchase Order: `purchase_order`
        - Consignment Order: `consignment_order`.
   1. New sequence for SO type **consignment order**: format **CXXXXX**
   1. Custom email templates
1. _Inventory modifications:_
   1. Modify the `_should_exclude_for_valuation` function (`stock.move`, `stock.move.line` and `stock.quant`) to ensure the saving of accounting entries **when the owner is set up** and is different from company.
   1. Create a configuration at product, product category and settings level to specify **input**, **output** and **valuation** accounts for consignment inventory.
   1. Ensure creation of inventory receipt and issue journal entries between warehouses (currently no journal entries are kept on transfers between 2 internal locations).

![consignment-order](/images/consignment-order.png)
> Consignment Order view example

## Replenishment order (RO)

Replenishment orders are used to replenish consignment goods. They should not be created manually or automatically to ensure internal system control.

They are created after the confirmation of a SO (`sale.order`, sale) of consignment products and their delivery, so they are associated with an inventory output (`stock.picking`, WH/OUT).

Their purpose is to generate a PO (`purchase.order`) addressed to the corresponding supplier in the inventory output.

### Details of the replenishment order development

1. _Purchase Order (`purchase.order`) model modifications:_
   1. If the type of the Purchase Order is `consignment_order`, a new boolean field `is_replenishment` should be displayed.
   1. If `is_replenishment` is true, a new selection field `replenishment_stage` should appear with the following options:
      - Open: `open` (order is active, but the replenishment has not yet been received from the supplier)
      - Partially replenished: `partially_replenished` (supplier delivered a part of the quantities ordered, but the replenishment is not complete)
      - Replenished: `replenished` (order has been completely fulfilled by the supplier, and the quantities ordered are already recorded in inventory)
      - Closed without replenishment: `closed` (order is closed **manually** because it is decided not to continue with the replenishment)
1. _Stock Picking (`stock.picking`) model modifications:_
   2. When replenishing goods, the system must ensure that the owner at receipt is the same as the owner at CO (lock `owner`).

![replenishment-order](/images/replenishment-order.png)
> Replenishment Order view example

## Consignment and replenishment accounting mapping

### 1. Goods receipt (goods receipt, WH/IN)

| Account | Description                                         | Debit              | Credit             |
| ------- | --------------------------------------------------- | ------------------ | ------------------ |
| 8135    | Goods on consignment (customer control)             | :heavy_check_mark: |                    |
| 8235    | Consignment goods offsetting entry (vendor control) |                    | :heavy_check_mark: |

### 2. Sales (product referral, WH/OUT)

| Account | Description                                              | Debit              | Credit             |
| ------- | -------------------------------------------------------- | ------------------ | ------------------ |
| 8136    | Consignment goods pending to be invoiced to the customer | :heavy_check_mark: |                    |
| 8135    | Goods on consignment                                     |                    | :heavy_check_mark: |

### 3. Billing

| Account | Description                         | Debit              | Credit             |
| ------- | ----------------------------------- | ------------------ | ------------------ |
| 6135    | Cost of goods sold                  | :heavy_check_mark: |                    |
| 8136    | Consignment goods pending invoicing |                    | :heavy_check_mark: |

### 4. Creation of replenishment type appropriation requests

| Account | Description                        | Debit              | Credit             |
| ------- | ---------------------------------- | ------------------ | ------------------ |
| 8135    | Goods on consignment               | :heavy_check_mark: |                    |
| 8235    | Consignment goods offsetting entry |                    | :heavy_check_mark: |

### 5. Supplier invoice record

| Account | Description                        | Debit              | Credit             |
| ------- | ---------------------------------- | ------------------ | ------------------ |
| 8235    | Consignment goods offsetting entry | :heavy_check_mark: |                    |
| 2205    | Suppliers (formal obligation)      |                    | :heavy_check_mark: |

# [:back:](../README.md)
