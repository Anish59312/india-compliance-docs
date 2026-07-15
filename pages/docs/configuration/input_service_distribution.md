---
description: Learn how to distribute common Input Tax Credit to your branches using the Input Service Distributor (ISD) mechanism in India Compliance, through the ISD Distribution Invoice and ISD Recipient Invoice.

og_title : Input Service Distribution (ISD) - India Compliance Documentation
og_url : https://docs.indiacompliance.app/docs/configuration/input_service_distribution

structured_data:
  - "@context": "https://schema.org"
    "@type": "WebPage"
    name: "Input Service Distribution (ISD) - India Compliance Documentation"
    description: "Learn how to distribute common Input Tax Credit to your branches using the Input Service Distributor (ISD) mechanism in India Compliance, through the ISD Distribution Invoice and ISD Recipient Invoice."
    mainEntityOfPage:
      "@type": "WebPage"
      "@id": "https://docs.indiacompliance.app/docs/configuration/input_service_distribution"
---

# Input Service Distribution (ISD)

The Input Service Distributor (ISD) mechanism lets a head office distribute the Input Tax Credit (ITC) on **common input services** to the branch GSTINs that benefit from them. India Compliance models this as **two documents** that mirror the way credit actually moves:

- **ISD Distribution Invoice** — the distributor (head-office / ISD) side. It records credit **leaving** the ISD GSTIN and how it is split across branches.
- **ISD Recipient Invoice** — the branch side. It records credit **received** by a branch GSTIN, and is the document your GST reports (GSTR-3B, ISD register) are built from.

::: tip How the split works
Credit is distributed to each branch in proportion to its **turnover**. During distribution the tax head can change based on where the branch is:

- **IGST** is always distributed as IGST.
- **CGST + SGST** stays as CGST + SGST when the branch is in the **same state** as the ISD.
- **CGST + SGST** is combined into **IGST** when the branch is in a **different state**.
:::

## Prerequisites

- Create a **Company Address** with **GST Category** set to **Input Service Distributor**. This is your distributor address. See [Setting Up](/docs/configuration/gst_setup) for updating GSTIN and address details.
- Make sure each recipient branch has an address with its own GSTIN and GST Category.
- Confirm the Company has a **Default ISD Provisional Account**. For Indian companies this account (named *ISD Distribution Provisional*) is created automatically. It acts as a routing account for distribution.
- **Distribute Expense with ISD Credit** in **GST Settings** is enabled by default, which distributes the taxable value along with the taxes of a purchase invoice. You may turn that off if needed.

> If you have items with ineligible ITC, it is recommended to create separate Purchase Invoices for them.

![Creating an ISD-applicable Purchase Invoice](./assets/isd_create_purchase_invoice.gif)

## From Purchase Invoice

Start from the Purchase Invoice and fan the credit out to several branches at once.

1. Open the submitted **Purchase Invoice** for the common input service. It is marked **Is ISD Applicable** automatically when the **Billing Address** has its GST Category set to **Input Service Distributor**.
2. Click **Create > ISD Distribution Invoices**.
3. In the **Distribute ITC to Recipient Branches** dialog:
   - The summary at the top shows the already distributed amounts.
   - Set the **Posting Date**.
   - Your branch addresses are pre-filled in the grid. You may add or remove as needed.
   - Add Turnover Amount for each address.
   - Remove the branches that are not getting services from this purchase invoice.
   - Branches with zero turnover will not be included in the distribution.

   > **Multi-company setup:** check **Is Against Party** in the dialog, then select the **Party Type**, **Party** (your internal Customer / Supplier), and the address you are distributing to.
4. Click **Create ISD Distribution Invoices**. One draft ISD Distribution Invoice is created per address.
5. Open each draft and review it:
   - The **Source Items** table breaks down the distribution by item. Expand any row to see the total and distributed amounts for each tax type.
   - The **Taxes** table shows the credit being reduced on the distributor side.
6. Once you are satisfied, **Submit** the invoice.
7. On submission you are asked to auto-submit the **ISD Recipient Invoice**. If you deny, a draft **ISD Recipient Invoice** will still be created.

   > **Multi-company setup:** for against-party distributions the Recipient Invoice is **not** auto-created. Use **Create > ISD Recipient Invoice** — India Compliance guesses the recipient values (company, addresses, and accounts). Verify them and **Submit**.

Use **View > Recipient Invoice** on the Distribution Invoice to open it.

![Distribute ITC to Recipient Branches dialog](./assets/isd_distribution_dialog.gif)

## Direct Invoice Creation

1. Go to the **ISD Distribution Invoice** list and create a new one.
2. Add the **Company**, **Distribution Address**, **Recipient Address**, and **Posting Date**.
3. Select the **Purchase Invoice** — this will autofill the **Source Items** table.
4. Add the **Recipient Branch Turnover** and **Total Turnover**. The **Distribution Ratio (%)** is calculated automatically.
5. Review the distribution in the **Source Items** table:
   - If **Distribute Expense with ISD Credit** is enabled, the expense is distributed according to the distribution ratio as well.
   - Otherwise only the taxes are distributed.
6. The **Taxes** table shows the impact of the distribution on the distribution company's taxes. (On the Recipient Invoice, it shows the impact on the branch.)
7. **Submit** the invoice. As in the distribution dialog flow, you are asked to auto-submit the **ISD Recipient Invoice**; if you deny, a draft is still created.

> **Multi-company setup:** check **Is Against Party** and select the **Supplier / Customer** with the respective address to distribute to. For against-party invoices you will **not** get the auto-create / auto-submit dialog — create the recipient side via **Create > ISD Recipient Invoice**, verify the guessed values, and **Submit**.

![Creating an ISD Distribution Invoice directly](./assets/isd_direct_invoice_creation.gif)

## External Company

When your ISD is registered outside the ERPNext ecosystem, directly create the ISD Recipient Invoice. Leave the **ISD Distribution Invoice Reference** blank, enter the **External ISD Invoice Number** to identify the source invoice, fill in the source items and taxes, and **Submit**.

![Creating an ISD Recipient Invoice for an external company](./assets/isd_external_branch.gif)

## Credit notes

To reverse a distribution, open the submitted **ISD Distribution Invoice** and click **Create > Credit Note**. The credit note distributes a negative amount in the same ratio as the original. The recipient-side credit note is created from the distribution-side credit note. You cannot reverse more than what was distributed.
