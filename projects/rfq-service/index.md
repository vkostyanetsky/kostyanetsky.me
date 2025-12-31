## 👀 Situation

A trading company (the client) runs a recurring request-for-quotation (RFQ) process. Before placing a customer order, the company contacts several suppliers, collects their prices, lead times, and terms, and then selects the most favorable offer.

## 💡 Objective

The challenge was that suppliers responded in whatever format was convenient for them: some by email, others via WhatsApp, and others in Google Sheets. A manager then had to manually consolidate this information and enter it into the ERP system. This consumed time, introduced errors, and led to disputes such as "did the supplier really send exactly this?".

The goal was to establish a more efficient way to collect quotations — without requiring suppliers to be trained to use a complex system.

## 💪 Actions

We decided to build a dedicated web interface for suppliers to submit quotations. The most obvious implementation path — exposing the ERP web client — was rejected immediately because it would require:

- publishing the ERP to the public internet;
- creating ERP accounts for all suppliers;
- purchasing additional client-connection licenses.

Instead, we built the quotation interface as an independent web application. The solution was split into two parts: ERP customization and development of the web application itself.

### 1. ERP scenario

To start an RFQ, a manager creates a document in the ERP: `Procurement Requisition`. It contains three datasets:

- suppliers (who will receive the request);
- items (what needs to be quoted);
- additional questions (terms, lead times, comments, and similar fields).

After the document is saved, the ERP generates a personal link for each supplier. The link works as an access token and is sent only to that supplier, making additional identity checks unnecessary.

![Procurement Requisition](document.png)

In addition, we implemented an HTTP service in the ERP that can return the data of a `Procurement Requisition` document and accept supplier responses, saving them as a `Response to RFQ` document.

### 2. Supplier UX

The web application is a simple form where a supplier can enter prices for each line item in the `Procurement Requisition` and answer additional questions from the client (delivery lead time, extra terms, and so on).

In practice, suppliers rarely complete the form in a single session, so we provided two actions:

- `Save as draft`: saves the current state of the form. The supplier can reopen the link later and edit the data. The client's managers do not see these values, because the quotation is not considered submitted at this stage.
- `Submit`: final submission. After this, the supplier's response becomes visible to the client's managers, and the supplier can no longer change the submitted data.

![UI](ui.png)

The frontend is built with React; the backend uses Flask.

The backend acts as a proxy between the web application and the ERP. The supplier opens their unique link, and the web application sends that token to the proxy. Using the token, the proxy requests the list of items to be quoted from the ERP and returns it to the web application.

With this approach, the web application does not need to know where the ERP is hosted or how to authenticate against it. The same applies to the next step, when the web application sends the quotation results back to the ERP.

![Architecture](schema.png)

It would have been possible to call the ERP HTTP service directly from the web application, but using a proxy eliminates the need to expose the ERP service to the public internet. It also simplifies secure authentication between the web application and the ERP service.

## 😎 Result

In the end, the RFQ process became a streamlined pipeline:

1. The client's manager creates an RFQ in the ERP;
2. The manager sends suppliers their personal links;
3. Suppliers respond through a simple, convenient interface;
4. Their responses are saved in the ERP (both drafts and final submissions).

Practical outcomes: manual consolidation across multiple channels was eliminated; data-entry errors decreased; quotations were received faster; and, critically, the solution required no supplier access to the ERP, no public exposure of any ERP interfaces, no additional ERP license costs, and no supplier account management.

A simplified version of the project can be deployed from the [GitHub repository](https://github.com/vkostyanetsky/RFQ).
