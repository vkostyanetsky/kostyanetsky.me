The daily award for most philosophical code goes to the author of this elegant way to check that two boolean variables are not equal to each other:

    If DataStructure.Property("AmountVATIn")
        And ((DataStructure.AmountVATIn And NOT SearchPriceIncludesVAT)
        OR (NOT DataStructure.AmountVATIn And SearchPriceIncludesVAT)) Then    
        Price = RecalculateAmountOnVATFlagsChange(Price, DataStructure.AmountVATIn, TabSectionLine.VATRate);
    EndIf;

I'm thinking of adding something like “And Not (DataStructure.AmountVATIn = SearchPriceIncludesVAT)” here to spice it up with a subtle note of insanity.