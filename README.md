# Multi-Line Insurance Policy & Claims Management System

Salesforce DX project for a complete insurance policy and claims management system built for the Smart India Hackathon 2025.

## Overview

This system implements a multi-line insurance platform supporting **Auto**, **Property**, and **Life** policies with automated quoting, premium calculation, claim routing, approval workflows, and an adjuster dashboard.

## Features

### Milestone 1: Core Data Model
- **Policy__c** custom object with 10 fields, 3 record types (Auto/Property/Life), 3 field sets, VIN validation rule
- **Claim__c** custom object with 6 fields, 3 record types (Accident/Property/Life)
- Auto-numbering: `P-{0000}` for policies, `C-{0000}` for claims

### Milestone 2: Policy Issuance & Claim Routing
- **AutoQuoting Screen Flow** - Guided policy creation with dynamic screens per policy type
- **PremiumCalculator Apex** - Invocable action with state-based logic (CA/TX/NY/FL/Other) and model year adjustments
- **Claim_Routing_Flow** - Record-triggered flow routing claims to queues by policy type

### Milestone 3: Claims Adjuster Dashboard
- **ClaimsAdjusterController Apex** - @AuraEnabled cacheable query with ClaimWrapper
- **claimsDashboardLwc** - Parent component with policy type filtering (All/Auto/Property/Life)
- **claimTileLwc** - Reusable child tile with icons, formatted currency, days open

### Milestone 4: Approvals, Security & Testing
- **High_Value_Claim_Approval** - 2-step approval (Senior Adjuster → Dept Manager) for claims >$50K
- **Submission_Automation_Flow** - Auto-submits high-value claims to approval process
- **Claim_Approver_Screen_Flow** - Approver UI with Approve/Reject + comments
- **Approve/Reject Claim Quick Action** - Flow-based action on Claim record page
- **ClaimsAdjusterControllerTest** - 234-line test class targeting 95%+ coverage
- **3 Permission Sets** - Insurance Agent Access, Claims Adjuster Access, Claims Manager Access
- **5 Sharing Rules** - Criteria-based by Policy_State__c (CA, TX, NY, FL, Other)

## Project Structure

```
force-app/main/default/
├── objects/
│   ├── Policy__c/          # Fields, record types, field sets, validation rules
│   └── Claim__c/           # Fields, record types
├── flows/                  # 4 Flow definitions
├── approvalProcesses/      # High_Value_Claim_Approval
├── quickActions/           # Approve_Reject_Claim
├── permissionSets/         # 3 permission sets
├── classes/                # 3 Apex classes + test
├── lwc/                    # 2 LWC components
└── sharingRules/           # 5 criteria-based sharing rules
```

## Deployment

### Prerequisites
- Salesforce CLI (`sf`)
- Dev Hub / Developer Edition org

### Deploy to Org
```bash
# Authenticate
sf org login web --alias insurance-org

# Deploy all metadata
sf project deploy start --target-org insurance-org
```

### Retrieve from Org (if built in UI)
```bash
sf project retrieve start \
  --target-org insurance-org \
  --metadata CustomObject,CustomField,RecordType,FieldSet,ValidationRule,Flow,ApprovalProcess,QuickAction,SharingRules,PermissionSet,ApexClass,LightningComponentBundle
```

## Testing

```bash
# Run all tests
sf test run local --target-org insurance-org

# Run specific test class
sf test run local --class ClaimsAdjusterControllerTest --target-org insurance-org
```

## Org Verification Checklist

After deployment, verify in org:
1. **AutoQuoting Flow** → Create Auto Policy (VIN: `1HGCM82633A123456`)
2. **Create Claim** → Amount: `60000` → Verify auto-submission to approval
3. **Approve/Reject Claim** button → Approve → Status = `Approved`
4. **Claims Dashboard** → Add to Lightning Page → Filter by policy type
5. **Permission Sets** → Assign to users
6. **Sharing Rules** → Verify state-based visibility

## Tech Stack

- **Platform**: Salesforce Lightning Platform
- **Language**: Apex, JavaScript (LWC)
- **Automation**: Screen Flows, Record-Triggered Flows, Approval Processes
- **UI**: Lightning Web Components
- **Testing**: Apex Test Classes
- **Deployment**: SFDX (source format)

## License

MIT License - Built for educational/hackathon purposes.