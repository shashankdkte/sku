# Out of Scope

The following items are **not included** in the current scope of this initial V2 release. However, they have been identified as potential enhancements and may be considered for inclusion in future versions of Sakura, based on priority, feasibility, and business needs.

> **Note:** Items marked with 🗺️ (ROADMAP) are planned for development in future versions of Sakura. While technically possible, they are not feasible to implement in the initial version due to time constraints. A roadmap outlining these plans will be shared separately.

---

## User Experience Features

### Shopping Cart Experience 🗺️

Users must submit access requests individually. Sakura does not support batching multiple requests in a shopping cart-style flow.

**Why:** This design decision is intentional due to the **risk of configuration mismatches** that may occur between the time a request item is added to the cart and when it is actually submitted. The system's underlying configurations, such as approvers, reports, or security models, can change at any time, which may result in inconsistencies and invalid records.

**Examples of issues:**
- The approvers assigned at the time a request was added to the cart may differ from those defined at the time of submission
- A request might have already been created and approved on behalf of the same user by someone else during the delay
- The requested object, such as a report, may have been deleted by a Workspace Owner before submission

### In-App Notifications 🗺️

The ability to display status updates and messages inside Sakura's user interface under a dedicated "Notifications" section is currently out of scope.

This functionality would include features such as:
- Read/Unread status tracking
- Indication of the originating action (e.g., request approved, rejected, delegated)
- Notification messages summarizing the event

### Smart Filters 🗺️

During the security dimension selections in the new request wizard, the elements shown on current step could be filtered based on the selection in previous steps. This is useful for scenarios like filtering unnecessary Cost Centers based on the selected service line.

---

## Bulk Operations

### Bulk Import of Any Kind (Approvers, Requests etc.)

Although bulk import is a valid and frequently requested feature, it bypasses the human-centered approval mechanisms that Sakura is built to enforce. Enabling bulk import risks undermining the system's purpose by allowing Workspace Admins (WSAs) to avoid using the standard access request process.

**Exception:** Before go-live, WSAs will be provided with a one-time option to manually load approvers and initial requests into the Sakura database. This is intended strictly for pre-production setup and not for ongoing use.

### Creating Requests for Multiple Users (On Behalf) 🗺️

Unlike Sakura V1, in V2 it is no longer possible to submit access requests on behalf of multiple users at once. Requests must be created individually per user.

**Why:** Each request involves validation steps for existing OLS and RLS access, which are highly user specific. Managing multiple users under one unified request would create complexity and ambiguity in how to handle different RLS/OLS contexts.

---

## Data Migration and Management

### Automatic Migration of V1 Data to V2

Sakura V1 and Sakura V2 operate on entirely different data models and functional architectures. As a result, automatic migration of existing data from V1 to V2, without human review or intervention, is not feasible.

**Support:** WSAs will be provided with a one-time opportunity before go-live to manually load V1 approvers and initial requests into the Sakura V2 database. This is intended only for initial setup and not for ongoing data transfer.

### Clone Rights of Existing Users 🗺️

The ability to create a new request for a user by copying the existing access rights of another user is currently out of scope. This is due to time constraints preventing its implementation in the first phase.

### Automated Invalidation of Relevant Records of Left Employees and Approvers 🗺️

Sakura does not automatically invalidate or delete records belonging to users who have left the organization.

**Why:** While employee status could theoretically be determined via the Workday dataset available in UMS, both the data quality and the data transfer process introduce significant risks. Inaccurate or incomplete information could lead Sakura to wrongly revoke valid access.

**Current behavior:** Departed users will naturally lose access once their Okta login is disabled.

**Future:** Sakura will include a dedicated page where users marked as "left" based on Workday data will be listed. Workspace Admins will have access to this page and can selectively revoke access for these users through manual, periodic review.

### Automated Remediation of Deleted Dimension Members 🗺️

When security dimension members (e.g., Entities, Markets, MSS, Clients) are deleted from the source system, Sakura does not automatically update or clean up related access mappings. Resolution of such issues requires manual intervention and validation by Workspace Admins.

---

## Approval Features

### SLA Enforcement for Approvers 🗺️

No SLA-based alerts, reminders, or escalations are implemented for access request approvals. If an approver delays or ignores a request, the system does not currently monitor or act on predefined timelines. This responsibility lies with the requesting user or admin to follow up manually.

### Advanced Approval Logic ("Any of" / "Both of") 🗺️

Sakura currently supports only **"Any Of"** logic, where approval from **any one** of the assigned approvers is sufficient to proceed. More complex scenarios, such as **"Both Of"** (requiring approval from **all** listed approvers), are not supported.

### Segregation of Duties in Approval Process 🗺️

The system does not enforce segregation of duties in the approval workflow. If there are multiple approvers assigned to a request, and one of them is also the person who initiated the request, Sakura will still allow that user to approve their own request.

### Approval Tree Traversing Using Multiple Security Dimensions 🗺️

Sakura does not currently support approver tree traversal based on combinations of multiple security dimensions. Although individual dimensions (such as Entity) can trigger hierarchical traversal to locate an approver, **cross-dimensional traversal** is not implemented.

---

## Security and Access

### Access without VPN 🗺️

Due to the confidentiality and criticality of Sakura's functional scope, remote users will be required to connect via VPN. On-premises users within the corporate network will have direct access without the need for a VPN. Public internet exposure is intentionally excluded from the current release.

### Security in Composite Datasets

Managing security where a dataset derives from or embeds another dataset with its own security context is not supported. Because in Composite Models, Row-Level Security from the published dataset still applies, it becomes difficult to determine and enforce a consistent RLS context.

### Only RLS Permission 🗺️

Requesting Row-Level Security (RLS) access for reports or applications whose Object-Level Security (OLS) is not managed by Sakura is currently out of scope.

**Why:** In Sakura, an OLS request is a prerequisite for any RLS request. This ensures that users first gain visibility to the reporting object (App or Report) before any data-level filtering is applied.

### Dimension-Based OLS Approver Workflows

OLS permission approval processes based on dimensions like Reports, Audiences, or similar will not be handled by Sakura. While OLS approvers can be defined at the SAR or Audience level, Sakura does not support routing approvals based on specific security dimension values.

**Example:** If Person A requests access to the **Finance Audience** with an RLS combination of **US - CXM**, the request will be sent to the **Finance Audience OLS approver**. There is no mechanism to assign different OLS approvers based on individual dimension values like "US" or "CXM."

---

## Data and Integration

### Undocumented Security Dimensions

Any security dimension not explicitly mentioned or defined within this document is considered out of scope.

### Non-Relational SQL Data Sources and Data

Dimensions that are not based on structured, relational data are excluded from the system's functional responsibilities. For instance, sharing a security dimension sourced from an Excel file will not be accepted, as **Excel is not a relational data source**.

### Security Dimension Management Within Sakura

Sakura will not handle the creation, maintenance, or administration of security dimensions; this must be done externally and be shared with Sakura.

### Data Quality Issues

Sakura assumes to receive clean and validated data; identifying or correcting data quality problems is out of scope.

### Data Mapping and Shaping

No functionality is provided for additional data transformation, mapping, or reshaping beyond what is already assumed in source datasets. Sakura will not include a mechanism to manipulate or transform incoming data from external systems into its own format.

### Source System Resiliency

Failures or inconsistencies caused by the reliability of the data source systems are not handled within Sakura.

### External Network Data Sources

Data sources that exist outside of the dentsu corporate network are not eligible for integration.

### Database-Level Data Exporting

Exporting data directly from Sakura databases to external destinations is out of scope. Sakura will prepare views in its database for sharing permissions-related data only, but it will not initiate or manage any data transfers.

### An API for Programmatic Access 🗺️

Currently, all actions within Sakura must be performed using the Sakura UI. There is no API available for external systems or users to trigger actions programmatically. This includes operations such as giving approvals, submitting requests, or exporting data.

An API layer may be considered in future phases for carefully scoped use cases.

### Cross-Domain Data Sharing

Inter-domain or platform-level data sharing capabilities (such as between business domains or environments) are not Sakura's responsibility.

---

## Technical Limitations

### Automatic RLS Remediation for Dataset Mismatches

The system does not automatically resolve Row-Level Security mismatches when reports using different datasets are added to an existing Audience. Resolution of this type of conflict is out of scope of Sakura. Responsibility for this integrity lies entirely with the App Owner.

### OLS for Standalone Reports 🗺️

Automatic management of Object-Level Security specific to non-App-based reports (standalone reports) will not be managed by the system.

---

## Summary

Many of these out-of-scope items are planned for future releases. The current V2 release focuses on core functionality to ensure a stable, secure, and well-governed access management system. Future enhancements will be prioritized based on user feedback and business needs.

### Roadmap Timeline

```mermaid
gantt
    title Sakura Feature Roadmap
    dateFormat YYYY-MM
    section V2 Release
    Core Functionality           :done, v2, 2025-01, 2025-08
    section Future Releases
    Shopping Cart                :future, cart, 2025-09, 2026-03
    API Access                   :future, api, 2025-12, 2026-06
    In-App Notifications         :future, notif, 2026-01, 2026-06
    Smart Filters                :future, filters, 2026-03, 2026-09
    SLA Enforcement              :future, sla, 2026-06, 2026-12
```

---

*[← Back to Common Functionality](08-common-functionality.md) | [Next: Assumptions, Risks, and Constraints →](10-assumptions-risks-constraints.md)*
