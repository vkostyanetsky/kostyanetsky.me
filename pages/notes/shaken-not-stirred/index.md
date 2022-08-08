Let's speak a bit about organization of the code. If you need to describe a set of objects with common properties, think about whether this description should be divided into separate methods, each of which intended to describe one specific object?

Let's look at this example — a method that describes tabular parts of documents suitable for some task:

    SupportedTypes["Document.SupplierPricesEntering"]   = "Prices";
    SupportedTypes["Document.OpeningBalancesEntering"]  = "CustomerAccounts,VendorAccounts";
    SupportedTypes["Document.Requisition"]              = "InventoryAndServices";

Everything seems fine, right? Descriptions are there; tabular parts are listed; splitting them by comma doesn't look expensive at all.

However, there are many documents in the method. Eventually, some colleague (or you) will need to refer to another tabular part of the document, which is already mentioned in the method. Something will distract they, they will forget to look for an existing line and something like this will turn out:

    SupportedTypes["Document.SupplierPricesEntering"]   = "Prices";
    SupportedTypes["Document.OpeningBalancesEntering"]  = "CustomerAccounts,VendorAccounts";
    SupportedTypes["Document.Requisition"]              = "InventoryAndServices";

    <...>

    SupportedTypes["Document.OpeningBalancesEntering"]  = "PayrollDeductions";

As a result, a part of description will be erased, and it's good if the related functionality is covered by tests.

Conclusion? Well, you can write a helper method that will take the document type and the name of **one** table part as input. The helper will add items to the SupportedTypes map and ensure that the data already added is not lost.

However, if you need a better solution, then consider doing as I wrote at the beginning of this note: split the method into auxiliary methods. One method contains description for one document only (for all its tabular sections). Something like:

    Procedure AddOpeningBalancesEnteringDocument(SupportedTypes)

        TabularSections = New Array;

        TabularSections.Add("CustomerAccounts");
        TabularSections.Add("VendorAccounts");
        TabularSections.Add("PayrollDeductions");

        SupportedTypes["Document.Requisition"] = StrImplode(TabularSections, ",");

    EndProcedure

What do we get here? Firstly, nobody will accidentally erase the description of the document. Secondly, SonarQube will be pleased: it is highly likely to begin swearing at repeating literals with the names of tabular parts, if the helper is implemented instead of code splitting.