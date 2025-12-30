## 👀 Situation

A trading company (our client) runs a regular request-for-quotation (RFQ) process. Before placing a customer order, their managers contact several suppliers to get prices, lead times, and terms, then pick the best offer.

## 💡 Task

The problem: suppliers replied in whatever format was convenient for them — some by email, some in WhatsApp, some in Google Sheets. Then a manager had to manually collect all that info and re-enter it into the ERP system.

Result: wasted time, typos, and the classic "Wait… Did the supplier really send that?" debates.

So we decided to build a supplier-facing web app — intentionally super simple (because nobody wants to train suppliers).

## 💪 Actions

We split the solution into two parts:

- extending the 1C configuration (create RFQs + receive replies);
- building the web app (the actual RFQ UI).

**1. The 1C flow**

In 1C, a manager creates a document called `Procurement Requisition` containing three data sets:

- suppliers (who we're sending to);
- items (what needs pricing);
- extra questions (terms, deadlines, comments, etc.).

After saving, 1C generates a personal link for each supplier. This link acts like an access token: it's sent only to that supplier, so extra login/password checks would be a bit of security theater.

![Procurement Requisition](document.png)

On the 1C side, we also implemented an HTTP service that:

- returns the `Procurement Requisition` data;
- accepts supplier replies and stores them as `Response to RFQ` documents.

**2. Supplier UX**

To avoid exposing the internal ERP web client to the internet — and to avoid creating supplier accounts inside 1C — we built a separate web application.

In practice it's a straightforward form where a supplier can:

- enter prices per line item from the `Procurement Requisition`;
- answer extra questions (delivery dates, terms, etc.).

In real life, suppliers rarely finish everything in one go, so we added two actions:

- Save as draft: saves progress. They can reopen the same link later and continue editing. The customer's managers don't see drafts because the quote isn't "official" yet;
- Submit: final send. After this, the supplier's response becomes visible to managers — and the supplier can't change it (no "oops I meant a different price" after the fact).

![UI](ui.png)

Frontend: React, backend: Flask. 

The backend works as a proxy between the web app and 1C:

- the web app receives the unique supplier link;
- sends it to the proxy;
- the proxy uses that link to fetch the RFQ items from 1C and returns them to the web app.

With this setup, the web app doesn't need to know where 1C is hosted, how to authenticate to it and how to submit results back.

![Schema](schema.png)

Yes, the web app could call 1C's HTTP service directly — but the proxy means we don't have to expose 1C interfaces to the public internet. Also, secure authentication between a public web client and 1C would be much more painful. The proxy basically acts like the bouncer at the club: "You can come in, but only through me" :)

## 😎 Result

In the end, RFQ processing turned into an actual pipeline:

1. The customer's manager creates an RFQ in 1C (Procurement Requisition);
2. Sends suppliers their personal links;
3. Suppliers respond in a clean, simple web UI;
4. Replies (both draft and final) are stored as `Response to RFQ` documents.

Practical impact:

- no more manual merging from email/WhatsApp/Sheets;
- fewer data-entry mistakes in 1C;
- faster quote turnaround;
- and most importantly: no supplier access to the internal 1C environment, no publishing internal components to the internet, no extra 1C licenses, and no supplier account management at all.

A simplified version of the project can be deployed from the [GitHub repository](https://github.com/vkostyanetsky/RFQ).
