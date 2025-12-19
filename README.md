flowchart TD
    Start([Start & Common Steps]) --> AddProducts[Add products to the shopping cart]
    AddProducts --> Checkout[Proceed to checkout]
    Checkout --> Shipping[Fill out shipping address form]
    Shipping --> Tokenized{Tokenized customer?}

    %% Branch A: Tokenized Customer
    Tokenized -- Yes --> DisplayOptionsA[Display two payment options]
    DisplayOptionsA --> SavedPayment[Use saved payment]
    SavedPayment --> TokenCharge{Token charge succeeds?}
    TokenCharge -- Yes --> PaySuccess1[Pay with token succeeds<br>Payment succeeds]
    TokenCharge -- No --> StepUp[Trigger step-up flow]
    StepUp --> Reauthorize[Handle fail reason & re-authorize payment on Klarna checkout]
    Reauthorize --> PaySuccess2[Payment succeeds]

    DisplayOptionsA --> ChangePayment[Change payment option]
    ChangePayment --> ChooseNew[Choose different payment option on Klarna checkout]
    ChooseNew --> ConfirmPay1[Confirm to pay]
    ConfirmPay1 --> PaySuccess3[Payment succeeds]

    %% Branch B: Not Tokenized Customer
    Tokenized -- No --> DisplayOptionB[Display single payment option: Pay with Klarna]
    DisplayOptionB --> Remember{Remember Klarna?}
    Remember -- Yes --> TokenCreation[Go to token creation flow]
    TokenCreation --> ConfirmPay2[Confirm to pay & authorize token creation on Klarna checkout]
    ConfirmPay2 --> PaySuccess4[Payment succeeds & token is created]

    Remember -- No --> OneTime[Standard one-time payment flow]
    OneTime --> ConfirmPay3[Confirm to pay on Klarna checkout page]
    ConfirmPay3 --> PaySuccess5[Payment succeeds]
