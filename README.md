# Temu-tokenization-project


```mermaid
sequenceDiagram
    participant User
    participant Merchant
    participant Klarna

    User->>Merchant: Add products to cart
    User->>Merchant: Proceed to checkout
    User->>Merchant: Fill shipping address

    Merchant->>Merchant: Is tokenized customer?

    alt Tokenized customer = Yes
        Merchant->>User: Display two payment options

        alt Choose saved payment option
            User->>Merchant: Select saved payment
            Merchant->>Klarna: Charge with token

            alt Token charge succeeds
                Klarna-->>Merchant: Charge success
                Merchant-->>User: Payment succeeds
            else Token charge fails
                Klarna-->>Merchant: Charge failed
                Merchant->>Klarna: Trigger step-up flow
                Klarna-->>Merchant: Re-authorization success
                Merchant-->>User: Payment succeeds
            end

        else Choose change payment option
            User->>Merchant: Change payment option
            Merchant->>Klarna: Redirect to Klarna checkout
            User->>Klarna: Select different payment option
            User->>Klarna: Confirm to pay
            Klarna-->>Merchant: Payment authorized
            Merchant-->>User: Payment succeeds
        end

    else Tokenized customer = No
        Merchant->>User: Display single option: Pay with Klarna
        Merchant->>Merchant: Remember Klarna?

        alt Remember Klarna = Yes
            Merchant->>Klarna: Start token creation flow
            User->>Klarna: Confirm payment and token creation
            Klarna-->>Merchant: Payment success + token created
            Merchant-->>User: Payment succeeds
        else Remember Klarna = No
            Merchant->>Klarna: Start one-time payment flow
            User->>Klarna: Confirm payment
            Klarna-->>Merchant: Payment authorized
            Merchant-->>User: Payment succeeds
        end
    end
