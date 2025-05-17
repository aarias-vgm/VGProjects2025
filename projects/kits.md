# Kits

## Class diagram

```mermaid
---
title: KIT CLASS DIAGRAM
---
%%{
  init: {
    "fontFamily": "monospace",
    "flowchart": {
      "htmlLabels": true,
      "curve": "linear"
    }
  }
}%%
classDiagram
   class SetTemplate {
      +Many2One~Product~ product
      +One2Many~SetLine~ quantities
      +One2Many~Set~ setLines
      +createKit(inventoryQuantities) kit
   }

   class SetLine {
      +int quantity
      +Product product
      +bool isRequired
      +bool isReusable
   }

   class Set {
      +string id
      +KitStatus status
      +Many2Many~Lot~ lots
   }

   class SetStatus {
   <<enum>>
   +READY
   +NOT_READY
   }

   SetTemplate "1" --> "*" SetLine : quantities
   SetTemplate "1" --> "*" Set : sets
   Set --> SetStatus

   note for SetTemplate "class mrp.bom"
   note for SetQuantity "class mrp.bom.line"
```

## Set up

- [x] 1. Create a kit template (**KitTemplate**) where the quantities of the products are determined.

- [ ] 2. Add the booleans `is_required` (required products) and `is_reusable` (products that will return to stock) to the [“KitQuantity”](#class-diagram) model.

![required](../images/kit-quantity-vars.png)
> Example of boolean vars on `mrp.bom.line`

- [ ] 3. Create the [“Kit”](#class-diagram)[^package] model with the following:

- Tracking ID
- _status_: preparation status with `ready` and `not_ready` values.
- List of batches (products).

- [x] 4. Validate in **Inventory** that there are sufficient batch quantities to assemble the kit (already implemented in the manufacturing module).

## Tracking

- [ ] 1. Create a new **Inventory** view where you can display the locations and in them the products and kits. The kits should look like drop-down lists of products with their quantities.

![required](../images/kit-inventory-view.png)
> Example of kits as droppable lists of products on custom view

## Inventory flow

```mermaid
---
title: NORMAL INVENTORY FLOW
---
%%{
  init: {
    "fontFamily": "monospace",
    "flowchart": {
      "htmlLabels": true,
      "curve": "linear"
    }
  }
}%%
flowchart LR

   input["Input"]
   stock["Stock"]

   subgraph WH ["WH-BA"]
      direction LR

      input --> stock
   end

   Vendor --> WH --> Customer
```

```mermaid
---
title: MEDICAL ASSETS INVENTORY FLOW
---
%%{
  init: {
    "fontFamily": "monospace",
    "flowchart": {
      "htmlLabels": true,
      "curve": "linear"
    }
  }
}%%
flowchart LR

   input["Input"]
   preparation["Preparation"]
   stock["Stock"]

   subgraph WH ["WH-BA"]
      direction LR

      input --> preparation --> stock
   end

   Vendor --> WH --> Customer
```

### Kits input

The kits are products that require a preparation process for their use. The inventory is conceived as a 2-step input inventory where (see above diagram):

- Most products will go from **In** to **Stock** automatically.
- By product or product category each of the components of the kits will go from **In** to **Preparation** automatically; after cleaning they will go manually to **Stock**.

### Kits output

The business model is based on charging mostly for the use on surgical purposes of the kit and very little for the purchase of the components of a kit.

At the inventory level, kits will be transported as a whole to customer locations from a warehouse called “Event”:

1. Prior to surgery, kits are transported to the customer location.
2. The use of the products specified on the expense sheet is charged.
3. After the surgery, the products with `is_reusable` = true and `is_required` = true that have not been purchased are returned.
4. The products arrive at the **Input** location of the warehouse from where they came from and by means of routing rules for _product_ or _product category_ are routed to **Preparation**.

```mermaid
flowchart LR
   subgraph Event["Warehouse 'Event'"]
      Client1
      Client2
      Client3
   end

   subgraph Client3["Loc: Client 3"]
      kit1(("Kit Column<br/>Surgery #1"))
      kit4(("Kit Column<br/>Surgery #4"))
   end

   subgraph Client2["Loc: Client 2"]
      A@{ shape: braces, label: "No kits" }
   end

   subgraph Client1["Loc: Client 1"]
      kit2(("Kit Column<br/>Surgery #2"))
   end

```

# [:back:](../README.md)

[^package]: Revisar módulo de paquetes en Odoo
