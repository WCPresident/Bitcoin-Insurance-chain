# Insurance Smart Contract

## Overview
This smart contract implements an insurance system on the Stacks blockchain. It allows users to initiate insurance policies, pay premiums, submit claims, and receive payouts. The contract also includes functionalities for policy cancellation, risk assessment updates, and maintaining policy history.

## Features
- **Initiate a New Insurance Policy**: Users can create an insurance policy with specified premiums and coverage.
- **Submit Premiums**: Premium payments activate or renew policies.
- **Submit Insurance Claims**: Insured parties can file claims within their coverage limits.
- **Approve and Release Claims**: Insurers can approve claims and release payouts accordingly.
- **Cancel Policies**: Insurers or insured parties can cancel policies and receive refunds if applicable.
- **Update Risk Assessments**: Insurers can assess risk scores, which may influence premiums.
- **Maintain Policy and Risk History**: The contract tracks payment history and risk scores over time.

## Data Structures
### Constants
- `grace-period`: The grace period before a policy expires (1,000 blocks).
- `max-coverage`: Maximum allowable insurance coverage (1,000,000 STX).

### Data Maps
- `insurance-policies`: Stores policy details for each insured party.
- `insurance-claims`: Tracks claims filed by insured parties.
- `policy-history`: Maintains historical premium payments and claim records.
- `risk-scores`: Stores risk assessment data for insured parties.

## Public Functions

### 1. Initiate a New Insurance Policy
```clarity
(define-public (initiate-policy (new-insurer principal) (new-insured-party principal) (premium-amount uint) (coverage-amount uint))
```
#### Parameters:
- `new-insurer`: Address of the insurance provider.
- `new-insured-party`: Address of the insured party.
- `premium-amount`: Premium cost in STX.
- `coverage-amount`: Maximum claimable amount.

#### Conditions:
- The insurer and insured cannot be the same.
- Coverage must not exceed `max-coverage`.
- The insured party must not already have an active policy.

#### Outcome:
- Stores the new policy and logs the creation event.

---
### 2. Submit Premium to Activate/Renew Policy
```clarity
(define-public (submit-premium (insured principal))
```
#### Parameters:
- `insured`: Address of the policyholder.

#### Conditions:
- The policy must exist and be inactive or due for renewal.

#### Outcome:
- Transfers premium from the insured to the insurer.
- Updates policy status to active.

---
### 3. Submit an Insurance Claim
```clarity
(define-public (submit-claim (insured principal) (claim-amount uint))
```
#### Parameters:
- `insured`: Address of the policyholder.
- `claim-amount`: Requested claim amount in STX.

#### Conditions:
- The policy must be active.
- The claim must not exceed remaining coverage.

#### Outcome:
- Records the claim and logs the event.

---
### 4. Approve a Submitted Claim
```clarity
(define-public (approve-claim (insured principal))
```
#### Parameters:
- `insured`: Address of the claimant.

#### Conditions:
- A claim must exist and be pending approval.

#### Outcome:
- Marks the claim as approved.

---
### 5. Release Payout After Claim Approval
```clarity
(define-public (release-payout (insured principal))
```
#### Parameters:
- `insured`: Address of the claimant.

#### Conditions:
- The claim must be approved.
- The payout must not exceed policy coverage.

#### Outcome:
- Transfers the payout to the insured and updates total claims.

---
### 6. Cancel Insurance Policy
```clarity
(define-public (cancel-policy (insured principal))
```
#### Parameters:
- `insured`: Address of the policyholder.

#### Conditions:
- The request must come from the insured or insurer.
- The policy must be active.

#### Outcome:
- Updates the policy status to inactive.
- Refunds a prorated amount if applicable.

---
### 7. Update Risk Assessment
```clarity
(define-public (update-risk-assessment (insured principal) (new-risk-score uint))
```
#### Parameters:
- `insured`: Address of the policyholder.
- `new-risk-score`: Risk score (1-100 scale).

#### Conditions:
- Only the insurer can update the risk assessment.
- The risk score must be within the valid range.

#### Outcome:
- Updates risk score and adjusts the premium accordingly.

## Helper Functions
- `adjust-premium-by-risk`: Adjusts premium rates based on risk scores.

## Event Logs
The contract logs significant events for transparency:
- `insurance-policy-created`: When a policy is initiated.
- `premium-paid`: When a premium is submitted.
- `claim-filed`: When a claim is submitted.
- `claim-approved`: When a claim is approved.
- `payout-released`: When a claim is paid.
- `policy-cancelled`: When a policy is terminated.
- `risk-assessment-updated`: When a risk assessment changes.

## Security Considerations
- **Validation Checks**: Ensures valid inputs and prevents unauthorized actions.
- **Ownership Verification**: Only policyholders or insurers can modify policies.
- **Funds Handling**: Uses `stx-transfer?` for secure transactions.
- **Event Logging**: Provides transparency and traceability.

## Future Enhancements
- Implementing **multi-signature approvals** for claims above a threshold.
- Integrating **oracle data** for dynamic risk assessments.
- Supporting **policy endorsements** and add-ons.


