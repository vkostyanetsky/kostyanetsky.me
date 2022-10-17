The more I explore the Bitrix24 REST interface, the more I am amazed at how different its developers mindsets are.

Let's take, for example, the interface of deals and product rows related to them. There is no amount field in the table of the products: like, you need the amount for each line – count it yourself, that's it. However, there is the amount field at the document level! Can you guess what the field is named?

AMOUNT? DEAL_AMOUNT? DOCUMENT_AMOUNT? AMOUNT_TOTAL?

You didn't guess, the correct answer is OPPORTUNITY.

![What the fuck?](what-the-fuck.jpg)

Where'd I digress? Yeah, a product line. It contains a product, a VAT rate, and a unit of measure. All three entities are completely independent: each has a separate table with auxiliary information and its own unique identifier. It is logical to assume that identifiers are stored in the product line: product ID, VAT rate ID, and unit ID.

Well, yes, but no. The product field actually contains ID, but for the VAT rate field it's a rate value. What's for the unit of measure field? Well, it contains a measurement code 🤷‍♂️

Database normalization? What? What does it mean? Get lost, bro, you're distracting us from work.