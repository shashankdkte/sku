![A red flower with black background AI-generated content may be
incorrect.](media/image1.png){width="1.1232874015748031in"
height="1.0957403762029747in"}

Sakura V2

Functional Design Document

Submitted

| Revisions |            |            |                    |
|-----------|------------|------------|--------------------|
| Version   | **Date**   | **Author** | **Change Summary** |
| 1.0       | 05.08.2025 | O. Ozturk  | Initial Version    |

| Signoffs        |            |                                           |                          |
|-----------------|------------|-------------------------------------------|--------------------------|
| Stakeholder     | **Date**   | **Status**                                | **Comments**             |
| Kate Rudwick    | 08.08.2025 | Awaiting Approval                         |                          |
| Nico Benga      | 08.08.2025 | Approved pending observations in comments | See comments in document |
| Nitin Menon     | 08.08.2025 | Awaiting Approval                         |                          |
| Safraz Hakamali | 08.08.2025 | Awaiting Approval                         |                          |
| Ken Lovingood   | 08.08.2025 | Awaiting Approval                         |                          |
| Patrick Sura    | 08.08.2025 | Awaiting Approval                         |                          |

# Contents {#contents .TOC-Heading}

[1. Overview [3](#overview)](#overview)

[1.1. Purpose of This Document
[3](#purpose-of-this-document)](#purpose-of-this-document)

[1.2. Scope of the Application
[3](#scope-of-the-application)](#scope-of-the-application)

[1.3. Architecture Diagram
[3](#architecture-diagram)](#architecture-diagram)

[1.4. Intended Audience and User Roles
[5](#intended-audience-and-user-roles)](#intended-audience-and-user-roles)

[1.5. Key Definitions [5](#key-definitions)](#key-definitions)

[1.6. Workspace Specific RLS Requirements
[10](#workspace-specific-rls-requirements)](#workspace-specific-rls-requirements)

[[1.6.1. \[TBW\] Requirements of FUM]{.mark}
[10](#tbw-requirements-of-fum)](#tbw-requirements-of-fum)

[1.6.2. Requirements of GI
[10](#requirements-of-gi)](#requirements-of-gi)

[1.6.3. Requirements of CDI
[11](#requirements-of-cdi)](#requirements-of-cdi)

[1.6.4. Requirements of WFI
[12](#requirements-of-wfi)](#requirements-of-wfi)

[1.6.5. Requirements of EMEA
[14](#requirements-of-emea)](#requirements-of-emea)

[1.6.6. Requirements of AMER
[16](#requirements-of-amer)](#requirements-of-amer)

[2. Functional Details by Role
[18](#functional-details-by-role)](#functional-details-by-role)

[2.1. Requester Role [18](#requester-role)](#requester-role)

[2.1.1. Requesting Access [18](#requesting-access)](#requesting-access)

[2.1.2. Viewing Existing Access
[23](#viewing-existing-access)](#viewing-existing-access)

[2.1.3. Notifications & Emails
[24](#notifications-emails)](#notifications-emails)

[2.2. Approver Role [25](#approver-role)](#approver-role)

[2.2.1. Types of Approvers
[25](#types-of-approvers)](#types-of-approvers)

[2.2.2. Approval Flow [25](#approval-flow)](#approval-flow)

[2.2.3. My Approvals Page [26](#my-approvals-page)](#my-approvals-page)

[2.2.4. Approval Detail Page for an Individual Request
[26](#approval-detail-page-for-an-individual-request)](#approval-detail-page-for-an-individual-request)

[2.2.5. Approval of an Individual Request via Email
[27](#approval-of-an-individual-request-via-email)](#approval-of-an-individual-request-via-email)

[2.3. Workspace Admin Role
[27](#workspace-admin-role)](#workspace-admin-role)

[2.4. Sakura Support Role
[28](#sakura-support-role)](#sakura-support-role)

[2.5. Sakura Administrator Role
[29](#sakura-administrator-role)](#sakura-administrator-role)

[3. Common Functionality
[30](#common-functionality)](#common-functionality)

[3.1. Delegations [30](#delegations)](#delegations)

[3.2. Traversing The Approver Tree Based on Security Dimension
Attributes
[30](#traversing-the-approver-tree-based-on-security-dimension-attributes)](#traversing-the-approver-tree-based-on-security-dimension-attributes)

[3.3. Auditing and Logging
[31](#auditing-and-logging)](#auditing-and-logging)

[3.4. Excel Export [32](#excel-export)](#excel-export)

[3.5. Access using OKTA and MS Entra
[32](#access-using-okta-and-ms-entra)](#access-using-okta-and-ms-entra)

[3.6. Data Sharing with External Parties
[32](#data-sharing-with-external-parties)](#data-sharing-with-external-parties)

[3.7. Data Imports for Security Dimensions
[33](#data-imports-for-security-dimensions)](#data-imports-for-security-dimensions)

[3.8. Emailing and Notifications
[33](#emailing-and-notifications)](#emailing-and-notifications)

[3.9. In-App Help [34](#in-app-help)](#in-app-help)

[3.10. Revocation [35](#revocation)](#revocation)

[4. Out of Scope [35](#out-of-scope)](#out-of-scope)

[5. Assumptions, Risks, and Constraints
[41](#assumptions-risks-and-constraints)](#assumptions-risks-and-constraints)

[5.1. Assumptions and Constraints
[41](#assumptions-and-constraints)](#assumptions-and-constraints)

[5.2. Risks [42](#risks)](#risks)

[6. Appendix [43](#appendix)](#appendix)

[6.1. Glossary And Acronyms
[43](#glossary-and-acronyms)](#glossary-and-acronyms)

[6.2. Diagrams [43](#diagrams)](#diagrams)

[6.2.1. Access Request Flow
[43](#access-request-flow)](#access-request-flow)

[6.2.2. Role Permission Matrix
[44](#role-functionality-matrix)](#role-functionality-matrix)

[6.3. Signoff References [45](#signoff-references)](#signoff-references)

# Overview

## Purpose of This Document

The purpose of this document is to define the functional design of the
Sakura application. It outlines the intended behavior of the system from
a business and end-user perspective, detailing the roles, workflows,
features, and rules that govern how access requests are submitted,
approved, and managed.

This document serves as a common reference point for stakeholders,
including product owners, developers, testers, and administrators. It
captures functional expectations to ensure that the implementation
aligns with the defined access control objectives, user experience
requirements, and operational boundaries of Sakura.

## Scope of the Application

Sakura is a self-service authorization management portal designed for
users within the dentsu organization to request, approve, and manage
access to Power BI reports and datasets. It supports both Object-Level
Security (OLS) and Row-Level Security (RLS) across multiple delivery
contexts, including Standalone Reports, Workspace Apps, and App
Audiences.

The application covers the full request lifecycle: from report/audience
selection and access configuration to multi-step approval flows
involving Line Managers, OLS Approvers, and RLS Approvers. It also
includes role-based administration, audit logging, delegation, in-app
help, and email notification features.

Sakura does not manage user access directly within Power BI. Instead, it
centralizes access request logic and approval workflows, and shares
finalized permission outputs with downstream systems through database
views.

## Architecture Diagram

The architecture of Sakura V2 consists of several integrated components
within the dentsu Azure and VPN environment. Each component plays a
specific role in supporting access request workflows, data integration,
and system operations.

**Sakura Portal**

The user-facing web application where end users submit access requests
and approvers review and take action on them.

**Okta + Azure AD**

The identity providers used for user authentication and access control.
All users must log in using their corporate credentials before accessing
the Sakura Portal.

**Users**

All authorized dentsu users who can log into Sakura to request access,
approve requests, or manage administrative configurations.

**Virtual Machine (Azure VM)**

The compute environment where the Sakura application and supporting
services run.

**Sakura Database (Azure SQL)**

The central database that stores all metadata related to access
requests, user roles, approvals, audit logs, and configuration.

**ETL (Azure Data Factory)**

Responsible for transferring Security Dimension Data from external
systems into the Sakura Database.

**Enterprise Data Platform (Microsoft Fabric)**

Provides Security Dimensions to Sakura, such as organizational
hierarchies, client lists, and other RLS-relevant structures.

In return, Sakura sends Approved RLS Requests back to EDP, which can be
used for further data access control or downstream processing.

**OLS Manager (Custom .NET Application)**

A utility interface used for managing object-level security mappings
such as report-to-audience relationships or manual overrides.

**Email Dispatcher (Custom .NET Application)**

Handles email notifications for request submissions, approvals,
rejections, and several other user-facing system actions.

![A screenshot of a computer AI-generated content may be
incorrect.](media/image2.png){width="4.079208223972003in"
height="4.8683409886264215in"}

Figure 1 - Architecture Diagram

## Intended Audience and User Roles

| **Role**             | **Description**                                                                                                        |
|----------------------|------------------------------------------------------------------------------------------------------------------------|
| Requester            | End user requesting access                                                                                             |
| Approver             | Reviews and approves/rejects access requests                                                                           |
| Workspace Admins     | Manages security models, approvers and object definitions within a Workspace                                           |
| Sakura Support       | Handles questions, technical issues, and helps users resolve blockers                                                  |
| Sakura Administrator | Configures the system, manages settings, automation, and audit features as well as Workspaces and Security Dimensions. |

## Key Definitions

![A screenshot of a computer AI-generated content may be
incorrect.](media/image3.png){width="3.745097331583552in"
height="4.557286745406824in"}

Figure 2 - Virtual Equivalents of Objects in Sakura and MS Fabric

Sakura is a system designed to manage both **Object-Level Security
(OLS)** and **Row-Level Security (RLS)** within Power BI environments.
Its architecture introduces a layer of abstraction over Microsoft Fabric
concepts, enabling centralized and user-friendly access control. Below
are the core concepts of Sakura, structured around OLS and RLS
components:

The first and foundational OLS component in Sakura is **Workspace**.
This corresponds directly to the Microsoft Fabric concept of a Power BI
Workspace, which holds various analytical assets, including reports,
datasets, applications, and associated security configurations. In
Sakura, a workspace serves as the container that governs all security
relationships and structures.

The second key OLS component is the **Workspace App**. Within a single
Workspace, multiple Workspace Apps can be created. These mirror Power BI
Apps in Fabric, which are essentially briefcases of reports grouped for
a specific business need.

In Sakura, each Workspace App is further subdivided into **Workspace App
Audiences**, which represent user groups that are granted visibility
into specific reports. These are functionally equivalent to Power BI App
Audiences in Fabric. Each Audience includes one or more reports made
available to that group.

From Sakura's perspective, there are two types of reports: **Workspace
Standalone Reports (SAR)** and **Workspace App Audience Reports (AUR)**.
Technically, both types are simply Power BI reports stored in a
workspace. However, they are differentiated by how they are shared with
users. If a report is shared directly with users without being packaged
in an App, it is categorized as a **SAR**. If it is distributed through
an App and linked to an Audience, it is considered an **AUR**.[^1]

![](media/image4.png){width="6.288888888888889in"
height="4.017361111111111in"}

Figure 3 - Report Delivery Scenarios and Methods

Beyond OLS, Sakura also manages RLS (Row-Level Security) components. The
central concept here is the **Workspace Security Model**. This defines
how RLS is structured within a Workspace, based on which dimensions
(i.e., business attributes) are applied to filter data visibility. A
Workspace may contain multiple semantic models (datasets), and each
model may either share or have distinct RLS configurations. For example,
a Finance Workspace might contain both "FIN" and "OFR" semantic models.
If both models rely on the same RLS dimension tables in the data
warehouse, Sakura allows them to share a single Workspace Security
Model. This simplifies user access management by preventing the need for
duplicate access requests across multiple models.

The final building block of RLS in Sakura is the **Security Model
Dimension**. These are the actual dimensions (such as Country, Brand, or
Department) used within a semantic model to define access filters. In
Microsoft Fabric, these correspond to the dimensions within the dataset
used for RLS filtering. In Sakura, they are modelled and managed
explicitly as part of the Workspace Security Model to allow for
fine-grained and auditable access control.

![A screenshot of a chat AI-generated content may be
incorrect.](media/image5.png){width="6.3in"
height="4.434722222222222in"}

Figure 4 - ER Diagram of Sakura Key Elements

Workspaces

The foundational unit in Sakura represents a Power BI Workspace. It
holds all analytical components such as reports, datasets (semantic
models), security models, and Workspace Apps. It is the starting point
for both OLS and RLS configurations in Sakura.

Currently there are following workspaces available in dentsu:

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 17%" />
<col style="width: 18%" />
<col style="width: 14%" />
<col style="width: 16%" />
<col style="width: 13%" />
</colgroup>
<thead>
<tr class="header">
<th>Fabric Workspace</th>
<th>Owner(s)</th>
<th>PBI Model</th>
<th>UMS Data Domain</th>
<th>PBI Designer</th>
<th>DWH Designer</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>DFI Workspace</td>
<td>Kate Rudwick / Terry Newell</td>
<td>FUM</td>
<td>Finance</td>
<td>Vitali/ Yadava</td>
<td>Sumon/ Sean</td>
</tr>
<tr class="even">
<td>Growth Insights Workspace</td>
<td>Nico Benga</td>
<td>Growth Insights Model</td>
<td>Growth Insights</td>
<td>Vitali/ Yadava</td>
<td>Amit</td>
</tr>
<tr class="odd">
<td>CDI Workspace</td>
<td>Nitin Menon</td>
<td>CDI Model</td>
<td>Not Applicable</td>
<td>Yadava</td>
<td>Sean</td>
</tr>
<tr class="even">
<td>Workforce Workspace</td>
<td>Safraz Hakamali</td>
<td>Workforce Model</td>
<td>Workforce, Timesheet</td>
<td>Alok</td>
<td>Alok</td>
</tr>
<tr class="odd">
<td>AMER Workspace</td>
<td>Ken Lovingood</td>
<td>ACM</td>
<td>Not Applicable</td>
<td>Pradeep</td>
<td>Dhiraj</td>
</tr>
<tr class="even">
<td>EMEA Workspace</td>
<td>Patrick Sura</td>
<td>EMEA market-specific models</td>
<td>Not Applicable</td>
<td>Alex (CE)<br />
Sara (FR)<br />
Simon (NOEUR)</td>
<td>Alex</td>
</tr>
</tbody>
</table>

Workspace Apps

A container within a workspace in Sakura, equivalent to a Power BI App
in Microsoft Fabric. Each Workspace App groups a collection of reports
for a specific business purpose and is the main distribution method for
reports to end users.

Workspace App Audiences

A grouping of reports within a Workspace App. Each audience represents a
distinct view configuration within the app, defining which reports are
visible to which group of people. Audiences do not define the users
themselves but instead define sets of reports that certain people are
allowed to see.

Workspace Reports

Workspace Reports refer to all Power BI reports hosted within Workspace
in Sakura, classified based on how they are delivered to end users.
There are two main types:

- **Workspace Standalone Reports (SAR):** These reports are shared
  directly with users without being included in any Workspace App. They
  are handled individually within Sakura's object-level security model
  and represent a direct access scenario. (See Method III & Method IV in
  *[Figure 2 - Report Delivery Scenarios and Methods]{.underline}*.)

- **Workspace App Audience Reports (AUR):** These reports are included
  within a specific Audience of a Workspace App and are delivered to
  users via that App structure.

Workspace Security Model

A structure that defines the RLS (Row-Level Security) logic for a given
Workspace. It specifies which dimensions are used for filtering and how
they apply to the semantic models within the Workspace. If multiple
semantic models share the same RLS setup, a single Workspace Security
Model is reused across them to avoid redundant access configurations.

Security Dimensions

Business dimensions (such as Entity, Service Line, Brand) that are used
to slice data in semantic models via RLS. In Sakura, these dimensions
form the backbone of the Workspace Security Model and control which rows
of data a user can see based on their assigned values.

RLS Requests

These are access requests initiated by end users based on a given
Workspace Security Model and its associated Security Dimensions. Users
specify the values they need access to (e.g., specific regions or
brands), and the request is evaluated against the RLS logic defined in
that model.

OLS Requests

Access requests related to object-level visibility, such as gaining
access to a Standalone Report or to a specific App Audience. These
requests trigger approval flows based on the target object's ownership
and configuration.

RLS Approvers

Designated individuals responsible for reviewing and approving RLS
Requests. Their approval authority is typically scoped to specific
values within a Security Dimension (e.g., a regional manager approving
access to their region's data).

OLS Approvers

Users responsible for approving OLS Requests. Their responsibility
depends on the context, for example, a report owner for Standalone
Reports or an audience-level approver for reports accessed via App
Audiences. The OLS approver logic in Sakura supports both report-based
and audience-based ownership models.

Report Catalogue

The Report Catalogue is the unified registry of all reports, audiences,
and apps across every Workspace within Sakura. It acts as a central
access point where users can view what content exists, regardless of
their permission status. Each item's type (report, audience, app) is
visually distinguished, allowing for intuitive understanding. The
catalogue includes a search function to help users quickly locate
content. Additionally, each item is visually marked to indicate whether
the user has access, enabling a clear and immediate understanding of
visibility rights.

## Workspace Specific RLS Requirements

The requirements in this section are based on the original *Sakura
Security Access -- Requirements Document*. However, after its
finalization, additional alignment meetings were conducted with
individual Workspace Owners to further clarify and revise the
expectations around Security Dimensions and RLS Workflows.

Each Workspace has unique data structures, governance rules, and access
patterns, which require tailored RLS configurations and Wizard UI logic.
This section consolidates the finalized requirements into two
subsections:

- **Security Dimensions**: Specifies which business attributes (e.g.,
  Market, Client, Practice Area) should be used to define access control
  per Workspace and confirm their availability in the UMS data model.

- **RLS Dimension Selection Workflow**: Describes how access requests
  should be guided, and the information is collected during the request
  creation.

### [\[TBW\] Requirements of FUM]{.mark}

#### Security Dimensions

#### RLS Dimension Selection Workflow

### Requirements of GI

Based on the meeting titled **"Sakura RLS for GI"** held on **21.07.2025
at 15:30 CET**, the finalized Security Dimensions for the **Growth
Insights Workspace** are as follows:

#### Security Dimensions

| Dimension                  | Dimension Attributes           | SL/PA Based | MSS Based |
|----------------------------|--------------------------------|-------------|-----------|
| BL_UMV.DimEntity           | Market -\> Cluster -\> Region  | x           | x         |
| BL_UMV.DimClient           | Client -\> Dentsu Stakeholder  | x           | x         |
| BL_UMV.DimServiceLine      | Service Line (filtered to PAs) | x           |           |
| BL_UMV.DimMasterServiceSet | L2 -\> L1 + Overall            |             | x         |

**Security Types**: SL/PA, MSS

#### RLS Dimension Selection Workflow

![A screenshot of a computer AI-generated content may be
incorrect.](media/image6.png){width="6.3in"
height="2.1444444444444444in"}

Figure 5 - RLS Workflow for GI Workspace

The RLS request workflow for the Growth Insights Workspace begins with
the selection of a Security Type. At this stage, users are presented
with two options:

- SL/PA (Service Line / Practice Area)

- MSS (Master Service Set)

Depending on the selected Security Type, the workflow follows one of the
two predefined paths. Both paths consist of three steps, differing only
in the second step.

**SL/PA Path:**

- **Step 1: Organization Selection**. The user is asked to choose an
  organization level: Market, Cluster, or Region. Based on the
  selection, the corresponding list is displayed. For example, if the
  user selects \"Cluster\", a list of available clusters is shown for
  selection.

- **Step 2: SL/PA Selection**. The user is presented with a
  tree-structured list showing only Practice Areas. After selecting a
  Practice, the flow continues.

- **Step 3: Client Selection**. The user chooses between All Clients and
  Specific Client.

  - If \"All Clients\" is selected, the process proceeds to the next
    step.

  - If \"Specific Client\" is selected, a list of Dentsu Stakeholders is
    shown, from which the user selects one stakeholder before
    continuing.

**MSS Path**

- **Step 1: Organization Selection**. Identical to the SL/PA path, the
  user selects one of: Market, Cluster, or Region, and sees the
  corresponding list.

- **Step 2: MSS Selection**. The user is shown a hierarchical list of
  MSS L1 and L2 elements. After selecting the appropriate MSS node, the
  flow continues.

- **Step 3: Client Selection**. Same logic as in the SL/PA path: the
  user selects either All Clients or a Specific Client, which triggers
  the Dentsu Stakeholder list if applicable.

### Requirements of CDI

Based on the meeting titled **"Sakura V2 RLS Dimensions for GI
Workspace"** held on **05.08.2025 at 10:00 CET**, the finalized Security
Dimensions for the **CDI Workspace** are as follows:

#### Security Dimensions

| Dimension             | Dimension Attributes                                         | CDI |
|-----------------------|--------------------------------------------------------------|-----|
| BL_UMV.DimEntity      | Legal Entity -\> BPC Entity-\> Market -\> Cluster -\> Region | x   |
| BL_UMV.DimClient      | Client -\> Dentsu Stakeholder                                | x   |
| BL_UMV.DimServiceLine | Service Line / PA                                            | x   |

**Security Types:** CDI (aka no Security Type selection is asked)

#### RLS Dimension Selection Workflow

![A diagram of a computer AI-generated content may be
incorrect.](media/image7.png){width="6.3in"
height="1.476388888888889in"}

Figure 6- RLS Workflow for CDI Workspace

The RLS workflow for the CDI Workspace is simplified, as it involves
only one Security Type. Therefore, there is no initial Security Type
selection. Instead, the user is guided through a fixed three-step
process to define access across two hierarchical dimensions:

- **Step 1: Organization Selection**. The user selects a level from
  Entity, such as Market, Cluster, or Region. Based on the selection,
  the relevant list is shown. For example, selecting \"Cluster\"
  displays the available clusters.

- **Step 2: Service Line Selection**. The user selects a Service Line or
  Practice Area from the tree structure.

- **Step 3: Client Selection**. The user chooses between All Clients and
  Dentsu Stakeholders.

  - If All Clients is selected, the process continues directly.

  - If Dentsu Stakeholder is selected, a list of stakeholders is shown,
    from which the user selects one.

### Requirements of WFI

Based on the meeting titled **"Sakura 2.0 - Workforce Model"** held on
**23.07.2025 at 15:00 CET**, the finalized Security Dimensions for the
**Workforce Insights Workspace** are as follows:

#### Security Dimensions

| Dimension                                   | Dimension Attributes                 | WFI |
|---------------------------------------------|--------------------------------------|-----|
| \[sensitive\].\[DimPeopleAggregatorNodeWD\] | Business Areas \> Business Functions | x   |
| BL_UMV.DimEntity                            | Market -\> Cluster -\> Region        | X   |

**Security Types:** WFI (aka no Security Type selection is asked)

#### RLS Dimension Selection Workflow

![A diagram of a diagram AI-generated content may be
incorrect.](media/image8.png){width="6.3in" height="1.94375in"}

Figure 7 - RLS Workflow for WFI Workspace

The RLS workflow for the Workforce Insights (WFI) Workspace is
simplified, as it involves only one Security Type. Therefore, there is
no initial Security Type selection. Instead, the user is guided through
a fixed two-step process to define access across two hierarchical
dimensions:

- **Step 1: Entity Selection**. The user is asked to choose an
  organization level: Market, Cluster, or Region. Based on the
  selection, the corresponding list is displayed. For example, if the
  user selects \"Cluster\", a list of available clusters is shown for
  selection.

<!-- -->

- **Step 2: People Aggregator Selection.** The user is then asked to
  choose a People Aggregator value from a hierarchical structure which
  has **Business Area** and underneath **Business Functions**, hiding
  the "No Function" elements from the tree. It should look like this:

> ![A screenshot of a white screen AI-generated content may be
> incorrect.](media/image9.png){width="2.108001968503937in"
> height="3.3320034995625547in"}

Figure 8 - WFI Wizard People Aggregator Tree View

### Requirements of EMEA

Based on the weekly regular meeting held on **25.07.2025 at 10:00 CET**,
the RLS configuration for the EMEA Workspace follows a multi-path
structure, where the user begins by selecting one of five Security
Types. Each Security Type corresponds to a distinct sequence of
dimension selections, as outlined below.

#### Security Dimensions

<table>
<colgroup>
<col style="width: 32%" />
<col style="width: 29%" />
<col style="width: 7%" />
<col style="width: 8%" />
<col style="width: 4%" />
<col style="width: 10%" />
<col style="width: 6%" />
</colgroup>
<thead>
<tr class="header">
<th>Dimension</th>
<th>Dimension Attributes</th>
<th>Orga</th>
<th>Client</th>
<th>CC</th>
<th>Country</th>
<th>MSS</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>BL_UMV.DimEntity</td>
<td>Legal Entity -&gt;<br />
BPC Entity -&gt;<br />
Market -&gt;<br />
Cluster -&gt;<br />
Region -&gt;<br />
Global</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td></td>
<td>x</td>
</tr>
<tr class="even">
<td>BL_UMV.DimServiceLine</td>
<td>Service Line -&gt; Overall</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td>x</td>
<td></td>
</tr>
<tr class="odd">
<td>BL_UMV.DimClient</td>
<td>Client -&gt; Dentsu Stakeholder -&gt; All</td>
<td></td>
<td>x</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td>BL_UMV.DimCostCenter</td>
<td></td>
<td></td>
<td></td>
<td>x</td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td>BL_UMV.DimCountry</td>
<td><p>Country-&gt; Cluster-&gt;</p>
<p>Region -&gt;</p>
<p>Global</p></td>
<td></td>
<td></td>
<td></td>
<td>x</td>
<td></td>
</tr>
<tr class="even">
<td>BL_UMV.DimMasterServiceSet</td>
<td>L4 -&gt; L3 -&gt; L2 -&gt; L1 - Overall</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td>x</td>
</tr>
</tbody>
</table>

**Security Types:** Orga -- SL/PA, Client, CC, Country, Orga - MSS

#### RLS Dimension Selection Workflow

![A diagram of a flowchart AI-generated content may be
incorrect.](media/image10.png){width="6.3in"
height="2.9868055555555557in"}

Figure 9 - RLS Workflow for EMEA Workspace

The RLS request workflow for EMEA Workspace begins with the selection of
a Security Type. At this stage, users are presented with two options:

- Orga - SL/PA (Service Line / Practice Area)

- Client

- CC

- Orga - MSS

- Country

Depending on the selected Security Type, the workflow follows one of the
two predefined paths. Both paths consist of three steps, differing only
in the second step.

**Orga -- SL/PA Path:**

- **Step 1: Organization Selection.** The user selects a level from
  Entity, such as Market, Cluster, or Region. Based on the selection,
  the relevant list is shown. For example, selecting \"Cluster\"
  displays the available clusters.

- **Step 2: Service Line Selection.** The user selects a Service Line or
  "Overall" from Service Line tree.

**Client Path:**

- **Step 1: Organization Selection.** The user selects a level from
  Entity, such as Market, Cluster, or Region. Based on the selection,
  the relevant list is shown. For example, selecting \"Cluster\"
  displays the available clusters.

- **Step 2: Service Line Selection.** The user selects a Service Line or
  "Overall" from Service Line tree.

<!-- -->

- **Step 3: Client Type Selection.** The user chooses between All
  Clients and Dentsu Stakeholders.

  - If **All Clients** is selected, the process proceeds to the next
    step.

  - If **Dentsu Stakeholder** is selected, the user is shown a list of
    stakeholders to choose from.

- **Step 4: Client Selection.** The user selects a stakeholder from the
  list (if applicable).

**CC (Cost Centre) Path:**

- **Step 1: Organization Selection.** The user selects a level from
  Entity, such as Market, Cluster, or Region. Based on the selection,
  the relevant list is shown. For example, selecting \"Cluster\"
  displays the available clusters.

<!-- -->

- **Step 2: Service Line Selection.** The user selects a Service Line or
  "Overall" from Service Line tree.

<!-- -->

- **Step 3: Cost Center Selection.** The user selects a Cost Center from
  the tree. The content of the tree changes based on the previous
  selection of Organization. For Levels upper than Legal Entity in
  Organization selection, only BPC Rollups will be shown. Single Cost
  Center and Business Unit elements are only available on Legal Entity
  level because they are legal entity specific.

**MSS Path:**

- **Step 1: Organization Selection.** The user selects a level from
  Entity, such as Market, Cluster, or Region. Based on the selection,
  the relevant list is shown. For example, selecting \"Cluster\"
  displays the available clusters.

<!-- -->

- **Step 2: MSS Selection.** The user navigates the hierarchy in MSS (L1
  to L4) and selects an MSS value.

**Country Path:**

- **Step 1: Country Selection.** The user selects a level from Geo
  Hierarchy, such as Country, Cluster, or Region.

- **Step 2: Service Line Selection.** The user selects a Service Line or
  "Overall" from Service Line tree.

### Requirements of AMER

Based on the meeting titled **"Sakura 2.0 - AMER implementation"** held
on **28.07.2025 at 14:00 CET**, the finalized Security Dimensions for
the **AMER Workspace** are as follows:

#### Security Dimensions

| Dimension              | Dimension Attributes                                                   | Orga | Client | CC  | PC  |
|------------------------|------------------------------------------------------------------------|------|--------|-----|-----|
| BL_UMV.DimEntity       | Legal Entity -\> BPC Entity-\> Market -\> Cluster -\> Region -\>Global | x    | x      | x   | x   |
| BL_UMV.DimServiceLine  | Service Line -\> Overall                                               | x    | x      | x   | x   |
| BL_UMV.DimClient       |                                                                        |      | x      |     | x   |
| BL_UMV.DimProfitCenter | Profit Center -\> Brand - All                                          |      |        |     | x   |
| BL_UMV.DimCostCenter   |                                                                        |      |        | x   |     |

#### RLS Dimension Selection Workflow

![A diagram of a diagram AI-generated content may be
incorrect.](media/image11.png){width="6.3in"
height="2.439583333333333in"}

Figure 10 - RLS Workflow for AMER Workspace

The RLS request workflow for the AMER Workspace begins with the
selection of a Security Type. At this stage, users are presented with
two options:

- Orga

- Client

- CC

- PC

Depending on the selected Security Type, the workflow follows one of the
two predefined paths. Both paths consist of three steps, differing only
in the second step.

**Orga -- SL/PA Path:**

- **Step 1: Organization Selection**. The user selects a level from
  Entity, such as Market, Cluster, or Region. Based on the selection,
  the relevant list is shown. For example, selecting \"Cluster\"
  displays the available clusters.

- **Step 2: Service Line Selection**. The user selects a Service Line or
  "Overall" from Service Line tree.

**Client Path:**

- **Step 1: Organization Selection**. The user selects a level from
  Entity, such as Market, Cluster, or Region.

- **Step 2: Service Line Selection**. The user selects a Service Line or
  "Overall" from Service Line tree.

- **Step 3: Client Type Selection**. The user chooses between All
  Clients and Dentsu Stakeholders.

  - If All Clients is selected, the process proceeds to the next step.

  - If Dentsu Stakeholder is selected, the user is shown a list of
    stakeholders to choose from.

- **Step 4: Client Selection**. The user selects a stakeholder from the
  list (if applicable).

**CC (Cost Center) Path:**

- **Step 1: Organization Selection**. The user selects a level from
  Entity, such as Market, Cluster, or Region.

- **Step 2: Service Line Selection**. The user selects a Service Line or
  "Overall" from Service Line tree.

- **Step 3: Cost Center Selection**. The user selects a Cost Center from
  the tree. The content of the tree changes based on the previous
  selection of Organization. For levels above Legal Entity, only BPC
  Rollups are shown. Single Cost Center and Business Unit elements are
  only available on Legal Entity level because they are legal entity
  specific.

**PC (Profit Center) Path:**

- **Step 1: Organization Selection**. The user selects a level from
  Entity, such as Market, Cluster, or Region.

- **Step 2: Service Line Selection**. The user selects a Service Line or
  "Overall" from Service Line tree.

- **Step 3: Profit Center Type Selection**. The user chooses between
  Profit Center or Brand.

- **Step 4: Profit Center Selection**. Based on the previous selection,
  a list of PCs or Brands is shown for selection.

- **Step 5: Client Type Selection**. The user chooses between All
  Clients and Dentsu Stakeholders.

- **Step 6: Client Selection**. If Dentsu Stakeholder is selected, the
  user selects a stakeholder from the list.

# Functional Details by Role

Each section below details all functionality available to the specific
role.

## Requester Role

### Requesting Access[^2]

Sakura offers a structured and intelligent wizard-based interface for
end users to request access to Power BI reports and datasets, supporting
both Object-Level Security (OLS) and Row-Level Security (RLS). The
request process is dynamic and adapts based on the report's delivery
method, existing access, and workspace configuration.

#### Requesting Access using Report Catalogue

1.  **User Initiates the Request**

    - The user begins by searching for a report they need access to
      using the built-in search functionality from the Report Catalogue.

    - The results will show clearly the type of report (SAR/AUR) and
      which App that report belongs to.

    - If there are multiple reports with the same name, they will be
      listed as two items.

2.  **Report Selection**

    - The user selects the desired report from the search results.

3.  **System Determines Report Delivery Method**

    - Sakura checks how the selected report is delivered:

      - **Standalone Report**

      - **Audience Report** (distributed via a Workspace App)

4.  **Branch 1: Standalone Report**

    - The system checks if the user already has access.

      - If access **exists**, the user is prompted whether they only
        want to change the RLS.

        - If "Yes", the process continues with RLS steps.

        - If "No", the wizard ends.

      - If access **does not exist**, Sakura identifies the OLS approver
        and proceeds.

5.  **Branch 2: App Audience Report**

    - The system finds all audiences associated with the report and
      lists them.

      - If **multiple audiences** are available, the user selects one.

    - The system checks if the user already has access to the selected
      audience.

      - If access **exists**, the user is asked if they only want to
        change the RLS.

        - If "No", the wizard ends.

      - If access **does not exist**, the system proceeds with OLS
        steps.

    - Sakura finds the applicable OLS Approver and stores the OLS
      permission in memory.

6.  **Security Model Selection**

    - The system lists all Security Models for the selected Workspace.

      - If only one model is found, Sakura auto-selects it.

      - If multiple models exist, the user chooses the desired one.

7.  **Check Existing RLS Access**

    - Sakura checks if the user already has RLS access to the selected
      Security Model.

      - If access exists, the user is asked if they would like to reuse
        existing RLS.

        - If "Yes", the wizard skips to summary.

        - If "No", proceed with custom RLS definition.

8.  **Security Type Selection**

    - The system lists available Security Types for the selected model.

    - The user selects a Security Type.

9.  **Security Dimension Selection**

    - Based on the selected Security Type, Sakura displays relevant
      Security Dimensions.

    - The user selects values from each dimension as needed.

10. **Approver Detection**

    - Sakura attempts to find an RLS Approver matching the selected
      combination of dimension values.

      - If no approver is found, the wizard ends.

      - If found, the request proceeds.

11. **Summary & Confirmation**

    - Sakura displays a summary view showing:

      - OLS and RLS request details

      - Any previously granted access.

    - The user clicks Finish to submit the request.

12. **Request Finalization**

    - The system creates the RLS and/or OLS permissions as needed and
      stores the request in the system.

    - Approval flow is triggered accordingly (Line Manager → OLS
      Approver → RLS Approver).

#### Requesting Access in Advanced Mode

Sakura's Advanced Mode offers a more direct way for experienced users to
submit access requests when they already know the technical structure of
the Workspace, App, Audience, or Report they need access to. Unlike the
Report Catalogue mode, this path allows users to bypass catalogue-driven
discovery and instead to select the OLS and RLS request contexts
directly.

1.  **User Initiates Advanced Request**

    - From the **Requests** menu, user selects **New Request
      (Advanced)**.

2.  **Workspace Selection**

    - The system displays a list of available Workspaces.

    - User selects the Workspace they want to access.

3.  **Object Selection**

    - User is asked to define the scope of access:

      - **Workspace App** + Audience

      - or a **Standalone Report (SAR)**

4.  **Delivery Method Logic**

    - Based on the selection:

      - If the selected item is a **SAR**, the system checks for
        existing access to that report.

      - If it is an **Audience**, system checks if the user already has
        access to that Audience.

5.  **Access Evaluation**

    - If access already exists, users are asked whether they want to
      request **only RLS** changes.

      - If user selects "No," the wizard ends.

      - If "Yes" or access does not exist, process continues.

6.  **OLS Approver Detection**

    - System finds the relevant OLS Approver.

    - OLS permission is stored in memory and process moves to RLS steps.

7.  **Security Model Selection**

    - The system lists all Workspace Security Models related to the
      selected object.

    - If only one exists, it is selected automatically.

    - If multiple, users choose one.

8.  **Check for Existing RLS Access**

    - The system checks if the user already has access to the selected
      model.

    - If yes, the user is asked if they want to use the existing RLS
      record.

      - If yes, wizard proceeds to summary.

9.  **Security Type and Dimension Selection**

    - User selects the **Security Type**.

    - System loads the related **Security Dimensions**.

    - User chooses the required dimension values.

10. **Approver Lookup**

    - Sakura tries to find the RLS Approver for the selected
      combination.

    - If no approver is found, the wizard ends.

    - If found, wizard proceeds.

11. **Summary and Finalization**

    - The system shows a summary screen with OLS and RLS request
      information.

    - User clicks **Finish**.

12. **Submission and Approvals**

    - The system creates OLS and RLS permission requests.

    - Standard approval flow begins: **Line Manager → OLS Approver → RLS
      Approver**

#### Other Request Wizard Functionalities

**Can Submit Multiple Access Requests (OLS or RLS)**

Sakura does not restrict users from submitting multiple access requests
for the same workspace. A user can submit:

- Multiple OLS requests (e.g., for different reports or audiences)

- Multiple RLS requests (e.g., for different security dimension
  combinations)

- Or any combination of both

Each OLS or RLS request is treated as a separate and independent access
request. Accordingly, each one triggers its own approval workflow.

During approval, approvers will be able to see the requester's existing
access rights within the same workspace.

**"Help Me" Option for Unsure Requesters**

At any step in the request wizard, users who are unsure how to proceed
can click the "Help me" button located on the page.

Depending on the context of the step:

- If the step already involves a known Workspace, the help request is
  automatically forwarded to the corresponding Workspace Owner. They are
  responsible for reviewing the help request and guiding the user
  accordingly.

- If the wizard step occurs before a Workspace has been determined
  (e.g., report search, before selection), the system forwards the help
  request to GoTo, prompting the user to create a Sakura App Support
  Case for further assistance.

This feature ensures that users do not get stuck or abandon their
request due to uncertainty, while routing the request to the right
responsible party based on where they are in the flow.

**Request on Behalf of Another User**

During one of the steps in the request wizard, the user is asked whether
the access request is being created for themselves or on behalf of
someone else.[^3]

- If the request is being created for another person:

  - The user submitting the request is recorded as Requested By

  - The person the request is intended for is recorded as Requested For

- If the request is for the user's own access, both values (Requested by
  and Requested For) are set to the same user.

This functionality is particularly useful in onboarding scenarios where
a buddy or line manager (LM) initiates access requests for a new joiner.

**Wizard Workflow Entry Points for Creating Requests (Guided,
Advanced)**

Sakura provides multiple entry points for users to initiate access
requests, depending on their level of certainty and familiarity with the
report structure.

There are two primary modes for creating requests, accessible via the
Requests menu:

- **New Request (Using Report Catalogue)**: This is the Guided mode. It
  walks the user step by step through the access request process,
  starting from report selection and proceeding through delivery method,
  OLS, and RLS definitions. For details, see section *[2.1.1.1
  Requesting Access using Report Catalogue]{.underline}* .

- **New Request (Advanced)**: This mode allows more experienced users to
  directly select the target Workspace App, or Report and manually
  define OLS and RLS details. For more on this method, refer to
  *[2.1.1.2 Requesting Access in Advanced Mode.]{.underline}*

Additionally, external report catalogues or documentation systems (such
as SharePoint, Confluence, or GoTo) can redirect users directly into
Sakura's Guided mode, with a specific report preselected. This is done
by appending parameters e.g. [reportTag]{.mark} to the URL, which Sakura
can use to pre-load the wizard with the correct context.

![A diagram of a report AI-generated content may be
incorrect.](media/image12.png){width="3.1782174103237097in"
height="3.1782174103237097in"}

There are no restrictions on how a request should be initiated. Whether
launched from within Sakura or from an external tool, users are free to
start wherever is most convenient, as long as the necessary identifiers
(e.g. [reportTag]{.mark}) are provided in the URL.[^4]

### Viewing Existing Access

Sakura provides two distinct views for users to monitor their current
access rights and the history of their access requests:

**My Access**

- Users can navigate to the My Access section from the main menu to view
  a list of all access rights currently granted to them.

- This includes both Object-Level Security (OLS) and Row-Level Security
  (RLS) permissions.

**My Requests**

- The My Requests section displays access request history where:

  - The user submitted the request for themselves, or

  - The user submitted the request on behalf of someone else, or

  - A request was submitted by someone else on behalf of the user.

- Records are categorized as OLS and RLS, and displayed separately.

**Request Details View**

- Clicking on any record opens a read-only detail page with:

  - Full request metadata

  - Approval status and audit history

- No actions can be performed on this page. However, contextual links
  are provided to navigate related items, such as:

  - The relevant Workspace App

  - The associated Audience or Standalone Report (SAR)

  - Linked Security Models for RLS records.

  - Etc.

### Notifications & Emails {#notifications-emails}

Sakura keeps requesters informed throughout the lifecycle of their
access requests via automated email notifications and in-app alerts.

**Email Notifications to Requesters**

- **Submission Confirmation**

  - Immediately after submitting a request, the requester receives a
    confirmation email including:

    - Request summary

    - Date of submission

    - A Reference for the Request

- **Approval Notifications**

  - The requester is notified when each approval step is completed (Line
    Manager, OLS, and RLS).

  - Emails indicate which step has been approved and who approved it.

- **Rejection Notifications**

  - If the request is rejected at any step, the requester receives an
    email with:

    - Rejection reason (mandatory field from approver)

    - Step at which it was rejected.

    - Guidance to resubmit if applicable.

- **Revocation Notifications**

  - If the request is revoked at any step, the requester receives an
    email with:

    - Revocation reason (mandatory field from approver)

    - Guidance to resubmit if applicable.

- **Delegation Awareness**

  - If an approver is unavailable and the request is approved by a
    delegate, the requester is still informed of who approved on their
    behalf.

## Approver Role

Sakura automatically identifies approvers based on their presence in
defined approval intersections. Anyone assigned as an approver for any
RLS or OLS context is considered an active **Approver** within the
system.

### Types of Approvers

There are three types of approval responsibilities within Sakura:

1.  **Line Managers**

    - Retrieved dynamically from Workday.

    - Represent the manager of the employee for whom the request was
      submitted.

2.  **OLS Approvers (Object-Level Security)**

    - Defined at the Workspace level for each report, audience, or app,
      depending on the report's delivery structure.

    - Maintained by one of the following roles: Workspace Owner,
      Workspace Technical Owner, or Workspace Approvals Owner.

    - **Rules for defining OLS Approvers:**

      - For **Standalone Reports** (when Delivery Mode is not
        *App-WithAudience*): approvers are defined in the Workspace
        Reports section.

      - If the report is part of an App:

        - If the Approval Mode in Workspace Apps is *AppBased*, the
          approver is defined directly in the Workspace Apps.

        - If Approval Mode is *AudienceBased*, the approver is defined
          in App Audiences.

3.  **RLS Approvers (Row-Level Security)**

    - Defined per Security Model and based on Security Dimensions.

    - Configured by Workspace Owner, Workspace Technical Owner, or
      Workspace Approvals Owner.

### Approval Flow

When a new access request is submitted, the system follows this
sequence:

1.  **Line Manager Approval**

    - An email notification is sent to the requester's line manager.

    - The line manager must approve before the request proceeds.

2.  **OLS Approver Approval**

    - After line manager approval, the OLS Approver receives a
      notification.

    - The OLS Approver must approve before the request proceeds

3.  **RLS Approver Approval**

    - Once OLS approval is completed, the RLS Approver is notified.

    - The RLS Approver must approve before the request finally saved as
      Approved.

![A screenshot of a computer AI-generated content may be
incorrect.](media/image13.png){width="6.3in"
height="2.4652777777777777in"}

Figure 11 - Approval Process

Each step must be approved in sequence. At every stage, approvers
receive email notifications with direct links to the approval task.
Sakura also performs security and anti-forgery validations before
rendering the approval interface.

During approval, approvers will be able to see the current step and
details in a progress timeline of approval sequence.

During approval, approvers will be able to see the requester's existing
OLS and RLS access rights within the same workspace.

Note that: Rejected requests **cannot be reopened or approved** later. A
new request must be created.

### My Approvals Page

The My Approvals section is available to all users in Sakura. However,
only those who are assigned as an approver for active or historical
requests will see entries listed on this page, which includes:

- **LM Approvals**

- **OLS Approvals**

- **RLS Approvals**

Each section provides:

- Filters to switch between **Approved** and **Awaiting Approval**
  requests.

- View-only access to historical approvals assigned to them during their
  active assignment period (past assignments are not visible)

- Delegated approvals (when active) will also appear here; for more
  information, see Section "5.1 Delegations"

Actions:

- **Approve** button to approve selected requests that are awaiting
  approval.

- **Reject** button with a **mandatory justification message** input to
  reject selected requests that are awaiting approval.

- **Revoke** button with a mandatory justification message input to
  revoke selected requests that are awaiting approval (just for OLS and
  RLS Approvers)

### Approval Detail Page for an Individual Request

When accessing an approval page individual request in Sakura UI, the
approval page includes:

- **Approval Chain Status** -- displays current state of each approval
  step.

- **Request Details** -- summarized OLS and RLS request information.

- **Existing Rights Overview** -- brief view of current OLS and RLS
  access for the user (with links to detailed views in a new tab)

- **Approval Actions**

  - **Approve** button.

  - **Reject** button with a **mandatory justification message** input.

  - **Revoke** button with a mandatory justification message input.

### Approval of an Individual Request via Email

Sakura will enable approvers to take action on access requests directly
via email.

When an approver is assigned to a request (Line Manager, OLS Approver,
or RLS Approver), the system sends a notification email that includes:

- Request summary details (user, object, dimensions, etc.)

- A direct **"Approve"** and **"Reject"** link.

- Current status of the approval chain

- Optional comments from the requester (if provided)

Upon clicking a link, the system performs the following:

- Validates the identity of the user (via session or login)

- Performs **anti-forgery and integrity checks** to prevent tampering.

- Redirects the user to a secure approval screen preloaded with request
  context.

Approvers can finalize their decision from this screen. If **Reject** is
selected, a justification message is required before submission.

This feature ensures that approvers can act quickly and securely, even
without navigating through the full Sakura interface.

## Workspace Admin Role

Users assigned as the Workspace Owner, Workspace Tech Owner to a
Workspace by Administrator, are granted administrative capabilities
specific to the configuration and management of Workspace elements
within the Sakura platform. This role plays a pivotal part in
maintaining the underlying security structure and configuring how Sakura
acts.

**Functional Capabilities:**

- **Security Model Administration**

  - Create and maintain Security Models per Workspace

  - Define the logic and associations that drive authorization.

- **Security Dimension Management**

  - Define and configure Security Dimensions for each Workspace.

  - Maintain mappings between Security Dimensions and Security Models

- **Standalone Report Management**

  - Add or edit Standalone Reports within Workspaces

  - Deactivate outdated or deprecated reports.

  - Map reports to appropriate Security Models

- **App & Audience Management**

  - Register and manage Apps published from Workspaces.

  - Create, edit, and deactivate App Audiences

  - Map App Audiences to relevant Security Models

- **App Audience Report Management**

  - Associate individual reports with App Audiences for contextual
    delivery.

- **Approver Configuration**

  - Assign and manage Object-Level Security (OLS) Approvers

  - Assign and manage Row-Level Security (RLS) Approvers for each
    dimension intersection.

- **Revocation of Access Requests**

  - Revoke existing OLS or RLS access requests that fall under their
    respective Workspaces.

  - This applies to permissions that were previously approved and are
    still active.

All actions are performed via an administrative UI with appropriate
validations, and every change is subject to audit tracking for
compliance and traceability.

A Workspace Owner will be simply responsible for defining the following
components in their workspace:

![A diagram of a software security system AI-generated content may be
incorrect.](media/image14.png){width="5.813654855643045in"
height="3.511134076990376in"}

Figure 12 - Components that are configured by WSA

## Sakura Support Role

The Sakura Support role is designed to provide visibility into the
system for assistance purposes without the ability to modify any data.
This role is strictly **read-only**, ensuring that support personnel can
investigate issues and guide users in support cases.

**Functional Capabilities (Read-Only)**

- **Request Visibility**

  - View all **RLS and OLS Requests** in the system.

  - Can not create, approve, reject, or modify any request.

- **Approver Overview**

  - See all defined **RLS and OLS Approvers**

  - Cannot edit, add, or remove any approvers.

- **Object-Level Definitions**

  - Access read-only views of:

    - Workspaces

    - Workspace Apps

    - Workspace App Audiences

    - Standalone Reports

    - Audience Reports

    - Security Models and Security Dimensions

- **System Emails**

  - View historical and scheduled **email messages** sent by the system
    for audit and support purposes.

- **Audit & Logs**

  - View **audit trails and logs** generated by user and system actions.

## Sakura Administrator Role

The Sakura Administrator holds the highest level of control and
system-wide configuration authority within the platform. While they do
not participate in access approvals, they have visibility into and
oversight over all Sakura functionalities.

**Functional Capabilities**

- **Global Permissions**

  - Can perform all actions available to other roles (e.g., Requesters,
    Workspace Admins), **except** approving requests.

- **Request Support**

  - Can append or inject RLS and OLS Approvers into existing approval
    workflows, even after request creation. This is useful in resolving
    stuck or misconfigured approval chains.

- **Workspace Management**

  - Add new Workspaces to Sakura

  - Deactivate (soft delete) Workspaces no longer in use.

- **System Configuration**

  - Enable or disable platform-wide system settings such as:

    - Emailing (turn notifications on/off)

    - Logging (control whether detailed logs are maintained)

- **Lookup Management**

  - Maintain system-wide **List of Values (LoVs)** for dropdowns,
    filters, and configuration screens.

- **Template and Content Management**

  - Manage and update:

    - **Email Templates** used for all automated communication.

    - **In-App Help Content** visible across Sakura interfaces

- **Audit and Logging**

  - Access all system logs and audit trails.

  - Export and share logs when necessary for compliance,
    troubleshooting, or support.

# Common Functionality

## Delegations  {#delegations}

Approvers can assign a delegate **only for themselves**, delegating on
behalf of another user is not permitted.

Only Approvers can delegate, other roles do not have delegation
abilities.

Delegation is **forward-looking**: it only applies to new requests
created during the delegation period and does **not** retroactively
affect existing or pending requests.

For example, if **Jane** is taking a sabbatical for the next three
months and wants **John** to act as her delegate, she can log into
Sakura, go to her **Profile Page**, and assign John under the
**Delegates** section. She can also assign multiple delegates by
entering their email addresses separated by semicolons (e.g.,
john@dentsu.com; joe@dentsu.com).

During the delegation period:

- Any new request where Jane is the designated approver will also
  include her assigned delegates as approvers.

- **Jane remains listed as an approver** so she can still view and act
  on requests created in her absence, if needed.

- **Any one of the listed delegates** can take the approval action, only
  **one** approval is required.

- Sakura supports only **"any of"** delegation logic. More complex
  scenarios like **"both of"** delegates needing to approve are
  currently **out of scope**. See section *"Out of Scope"* for more
  information.

## Traversing The Approver Tree Based on Security Dimension Attributes

Sakura includes a traversal mechanism within its approver-finding
algorithm for **Security Models that use "Entity" as a Security
Dimension**. This mechanism ensures that if no direct approver is found
for a given request, the system will attempt to locate an approver at
higher levels of the organizational hierarchy.

**How it works:**

- When a request is submitted with a specific **Entity** selection
  (e.g., a Market like "Germany"), Sakura first checks for an exact
  approver match at that most granular level.

- If no approver is found, the system **traverses upward** in the
  predefined entity tree:

  - From Market (e.g., *Germany*)

  - To Cluster (e.g., *DACH*)

  - To Region (e.g., *EMEA*)

  - And finally, to the *Global* level

- At each level, Sakura attempts to match the selected Security
  Dimension combination to a defined approver.

**Failure Scenario:**

- If no approver is found at **any** level of the entity hierarchy, the
  algorithm returns a **"NOT FOUND"** result.

- In such cases, the request will **not proceed**, permission creation
  is blocked until an appropriate approver is defined.

It is applicable **only to Entity-based Security Models**; other
dimensions do not benefit from hierarchical fallback.[^5]

## Auditing and Logging

Every administrative or user-driven change (e.g., request submission,
approval actions, configuration edits) is logged in an auditable,
timestamped format.

Key audit features:

- Change logs per object (Report, Audience, Security Model etc.)

- Historical records of granted and revoked access

- Exported Records using Excel Export Functionality

- View Action for Requests

- Traceability of all approval and delegation actions

- View and exportable audit trails for compliance purposes

Simple Sakura logs everything related to who did what, when and how.

## Excel Export

Within Sakura, exporting to excel is by default forbidden. However, due
to various business reasons, Sakura will be providing Excel exports for
various tables:

- RLS Requests

- OLS Requests

- RLS Approvers

- Workspace

- Workspace Apps

- Workspace App Audiences

- Workspace App Reports

All exports are governed by user permissions and traceable through audit
logs.

## Access using OKTA and MS Entra

Authentication and access to Sakura is managed via SSO, supporting KTA
and Microsoft Entra ID. User context is validated via secure tokens.

## Data Sharing with External Parties[^6]

Sakura Database exposes **read-only views** of security and permissions
data for integration with other systems. However:

- Sakura does **not push** any data externally.

- External systems are responsible for **pulling data** from these
  prepared views.

- Only permissions-related metadata (not report data or datasets) is
  shared.

At the end of the access request lifecycle, Sakura produces two types of
finalized outputs for each user:

RLS (Row-Level Security) List per Security Model and Security Type for
Each WS

RLS definitions capture the specific data-level access granted to the
user across models and security types. These are interpreted by the
downstream system for enforcing data visibility. Example output for a
user (e.g., Jane):

When Jane opens a report, she can view data where she has approved
access to:

- Germany CXM as Orga,

- and/or Germany Media as Orga,

- and/or Germany Creative Mercedes as Client,

**RLS Data Sharing Format:**

| Dimension I | Dimension II | Dimension \... | Dimension n | Requested For   | Security Type      | Approved By         | Approval Date       |
|-------------|--------------|----------------|-------------|-----------------|--------------------|---------------------|---------------------|
| Value I     | Value II     | Value \...     | Value n     | user@dentsu.com | SL/MSS/Client etc. | approver@dentsu.com | dd.MM.yyyy hh:mm:ss |

OLS (Object-Level Security) List per Report, App, or Audience for Each
WS

OLS definitions capture which reports, applications, or audiences the
user is allowed to open. Example output for the same user:

Jane has access to reports that are:

- under Finance Audience in Samurai App,

- and/or in DWI App,

- and/or a standalone report in CDI Workspace.

**OLS Data Sharing Format:**

| Catalogue Item Type | Catalogue Item   | Workspace App | Requested For   | Approval Date       |
|---------------------|------------------|---------------|-----------------|---------------------|
| Report              | Report Details   | App Details   | user@dentsu.com | dd.MM.yyyy hh:mm:ss |
| Audience            | Audience Details | App Details   | user@dentsu.com | dd.MM.yyyy hh:mm:ss |

## Data Imports for Security Dimensions

Security Dimensions are not created or maintained within Sakura.
Therefore, the necessary data must be imported into Sakura from external
systems. Sakura performs these imports periodically using Microsoft
Azure Data Factory (ADF).

All imported Security Dimensions are subject to historization and
versioning to ensure full traceability and auditability.

In addition to Security Dimensions, **Line Manager** information is also
imported from UMS. This data is used in the approval process to
dynamically determine the requester\'s line manager.

## Emailing and Notifications

Sakura communicates key events to users via email, leveraging a
standardized notification system that is fully configurable through
system settings.

**Delivery Scope**

- Sakura can send emails **only to dentsu-owned domains**, as restricted
  by the SMTP relay server configuration.

**Configurable Parameters**

Administrators can configure the following email settings via the system
configuration panel:

- Whether emails should be sent or suppressed

- Email frequency and retry attempts.

- Sender address

- Email subject tags (prefixes)

**Email Triggers**

Sakura sends emails for the following events:

- When a request is created: notifications are sent to both the person
  who created the request (*RequestedBy*) and the person the request was
  created for (*RequestedFor*).

- When a request requires approval: notifications are sent to the
  designated Approvers.

- When a request is approved or rejected: both *RequestedBy* and
  *RequestedFor* are informed.

- When a user is assigned as an Approver: notification is sent based on
  the relevant system setting.

**Templates and Format**

- All emails are generated using predefined, standardized templates to
  ensure consistency in tone and structure.

**Retry and Logging Mechanism**

- Sakura retries sending undelivered emails a configurable number of
  times, as defined in the system settings.

- Both pending and successfully delivered emails are logged and stored
  in a dedicated internal table for traceability and auditing purposes.

## In-App Help

Sakura provides contextual, in-application help to guide end users in
understanding various interface elements and actions.

**Functionality Overview:**

- Help is delivered through information bubbles that appear when users
  hover over specific elements on the page with their mouse.

- These tooltips provide short, descriptive explanations about the
  purpose or behavior of the corresponding UI element.

**Configuration:**

- Help bubbles can be configured for any UI element on any page within
  Sakura.

- The help content is defined and managed exclusively by Sakura
  Administrators, typically upon request from stakeholders or users.

Through this mechanism Sakura tries to enhance usability and reduce the
number of questions for end users by offering lightweight, embedded
guidance directly within the application.

## Revocation

Revocation refers to the cancellation of an existing access request that
is currently in an Approved (or earlier) state.

**Key Rules and Behavior:**

- Once a request is revoked, it is permanently cancelled and cannot be
  reactivated. If access is still required, a new request must be
  submitted.

- Revocation can only be performed by users with one of the following
  roles:

  - Approver

  - Workspace Admin

  - Sakura Administrator

- Revocations are done per RLS or OLS separately.

**Permissions by Role:**

- Approvers and Workspace Admins can only revoke requests within their
  own authorization scope (e.g., for requests they were responsible for
  approving).

- The Sakura Administrator role has global authority and can revoke any
  request regardless of scope.

**Audit and Notifications:**

- A revocation reason is mandatory and must be provided during the
  revocation process.

- This reason, along with the identity of the user who performed the
  revocation, is:

  - Logged on the request record.

  - Included in the email notification sent to the following parties:

    - To: Requested For

    - CC: Requested By (if different than Requested For)

    - CC: Line Manager (if different than Requested By)

# Out of Scope[^7]

The following items are not included in the current scope of this
initial V2 release. However, they have been identified as potential
enhancements and may be considered for inclusion in future versions of
the Sakura, based on priority, feasibility, and business needs.

**Shopping Cart Experience**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

Users must submit access requests individually. Sakura does not support
batching multiple requests in a shopping cart-style flow.

This design decision is intentional due to the **risk of configuration
mismatches** that may occur between the time a request item is added to
the cart and when it is actually submitted. The system\'s underlying
configurations, such as approvers, reports, or security models, can
change at any time, which may result in inconsistencies and invalid
records.

For example:

- The approvers assigned at the time a request was added to the cart may
  differ from those defined at the time of submission.

- A request might have already been created and approved **on behalf of
  the same user** by someone else during the delay.

- The requested object, such as a report, may have been **deleted** by a
  Workspace Owner before submission.

While each of these scenarios could theoretically be handled with
advanced logic and validations, they require **complex solutions**, and
increase system and workflow complexity. Therefore, the shopping cart
concept has been **deferred to a future phase** of the project for
better planning and implementation.

**Bulk Import of Any Kind (Approvers, Requests etc.)**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

Although bulk import is a valid and frequently requested feature, it
bypasses the human-centered approval mechanisms that Sakura is built to
enforce. Enabling bulk import risks undermining the system\'s purpose by
allowing Workspace Admins (WSAs) to avoid using the standard access
request process. Over time, this can lead to misuse of the feature,
where bulk import is used as a substitute for proper, user-driven
requests. This behavior poses a threat to the system\'s adoption and
overall governance integrity.

For this reason, bulk import will not be included in the initial phase
of Sakura. It may be reconsidered for later phases, provided that
appropriate safeguards are designed.

As an exception, before go-live, WSAs will be provided with a one-time
option to manually load approvers and initial requests into the Sakura
database. This is intended strictly for pre-production setup and not for
ongoing use.

**Automatic Migration of V1 Data to V2 (Requests, Approvers etc.)**

Sakura V1 and Sakura V2 operate on entirely different data models and
functional architectures. As a result, automatic migration of existing
data from V1 to V2, without human review or intervention, is not
feasible. Any such migration must be handled manually and is the
responsibility of the Workspace Admins (WSAs).

To support the transition, WSAs will be provided with a one-time
opportunity before go-live to manually load V1 approvers and initial
requests into the Sakura V2 database. This is intended only for initial
setup and not for ongoing data transfer.

**Clone Rights of Existing Users**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

The ability to create a new request for a user by copying the existing
access rights of another user is currently **out of scope**. This is due
to two reasons: it is not considered a core functionality for the
initial release, and there are **time constraints** preventing its
implementation in the first phase. However, this feature is recognized
as useful and may be planned for **future versions** of Sakura.

**Automated Invalidation of Relevant Records of Left Employees and
Approvers** ![](media/image15.png){width="0.3951388888888889in"
height="0.1375in"}

Sakura does not automatically invalidate or delete records belonging to
users who have left the organization. For example, if a departed
employee was an approver, a WS Tech Admin, or had active access
requests, those records will remain unchanged in the system. No
automated removal is triggered.

While employee status could theoretically be determined via the Workday
dataset available in UMS, both the data quality and the data transfer
process introduce significant risks. Inaccurate or incomplete
information, whether due to unreliable Workday records or a production
issue during integration, could lead Sakura to wrongly revoke valid
access, potentially affecting large sets of users and forcing them to
re-request everything from scratch. This could result in serious
disruption and confusion across the system.

To mitigate these risks, automated invalidation has been excluded from
the initial release. Instead, departed users will naturally lose access
once their Okta login is disabled, which prevents them from accessing
both Sakura and Power BI.

In a future phase, Sakura will include a dedicated page where users
marked as \"left\" based on Workday data will be listed. Workspace
Admins will have access to this page and can selectively revoke access
for these users through manual, periodic review.

**Automated Remediation of Deleted Dimension Members**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

When security dimension members (e.g., Entities, Markets, MSS, Clients)
are deleted from the source system, Sakura does not automatically update
or clean up related access mappings. This may lead to errors in request
processing or broken access links. Resolution of such issues requires
manual intervention and validation by **Workspace Admins**, who are
responsible for maintaining integrity between dimension values and
access definitions.

**SLA Enforcement for Approvers**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

No SLA-based alerts, reminders, or escalations are implemented for
access request approvals. If an approver delays or ignores a request,
the system does not currently monitor or act on predefined timelines. As
a result, there is no automated follow-up, delegation trigger, or
escalation path to ensure timely processing of pending approvals. This
responsibility lies with the requesting user or admin (Workspace/App) to
follow up manually.

**Advanced Approval Logic (\"Any of\" / \"Both of\")**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

Sakura currently supports only **"Any Of"** logic, where approval from
**any one** of the assigned approvers is sufficient to proceed. More
complex scenarios, such as **"Both Of"** (requiring approval from
**all** listed approvers), are not supported.

**Segregation of Duties in Approval Process**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

The system does not enforce segregation of duties in the approval
workflow. If there are multiple approvers assigned to a request, and one
of them is also the person who initiated the request, Sakura will still
allow that user to approve their own request. There are no system-level
checks or blocks to prevent self-approval.

**Access without VPN**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

Due to the confidentiality and criticality of Sakura's functional scope,
and considering that the security audit process may be extensive, remote
users will be required to connect via VPN. On-premises users within the
corporate network will have direct access without the need for a VPN.
Public internet exposure is intentionally excluded from the current
release and will be considered in a future version, pending successful
completion of security assessments.

**Security in Composite Datasets**

Managing security where a dataset derives from or embeds another dataset
with its own security context is not supported. Because in Composite
Models, Row-Level Security from the published dataset still applies, it
becomes difficult to determine and enforce a consistent RLS context.

**Undocumented Security Dimensions**

Any security dimension not explicitly mentioned or defined within this
document is considered out of scope.

**Non-Relational SQL Data Sources and Data**

Dimensions that are not based on structured, relational data are
excluded from the system's functional responsibilities. For instance,
sharing a security dimension sourced from an Excel file will not be
accepted, as **Excel is not a relational data source**. The system
assumes dimensions are maintained in well-structured, queryable
SQL-based systems that support referential integrity and dynamic
updates.

**Security Dimension Management Within Sakura**

Sakura will not handle the creation, maintenance, or administration of
security dimensions; this must be done externally and be shared with
Sakura.

**Data Quality Issues**

Sakura assumes to receive clean and validated data; identifying or
correcting data quality problems is out of scope.

**Data Mapping and Shaping**

No functionality is provided for additional data transformation,
mapping, or reshaping beyond what is already assumed in source datasets.
**Sakura will not include a mechanism to manipulate or transform
incoming data from external systems into its own format.** Instead, it
is expected that the source systems deliver data that is already shaped
and structured according to Sakura's requirements. The responsibility
for aligning data structure and semantics lies with the system providing
the data, not with Sakura.

**Source System Resiliency**

Failures or inconsistencies caused by the reliability of the data source
systems are not handled within Sakura.

**External Network Data Sources**

Data sources that exist outside of the dentsu corporate network are not
eligible for integration.

**Database-Level Data Exporting**

Exporting data directly from Sakura databases to external destinations
is out of scope. **Sakura will prepare views in its database for sharing
permissions-related data only**, but it will not initiate or manage any
data transfers. Other systems may access this data by **pulling from the
shared views**; however, Sakura itself will not push data to any
external destination or service.

**An API for Programmatic Access**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

Currently, all actions within Sakura must be performed using the Sakura
UI. There is no API available for external systems or users to trigger
actions programmatically. This includes operations such as giving
approvals, submitting requests, or exporting data.

For example, a system cannot send an approval decision on behalf of an
approver using an API. Similarly, external tools cannot extract data
from Sakura through an API interface. Instead, Sakura provides read-only
database views for sharing permission-related data, which external
systems may pull from, but no push-based integration or API access is
currently supported.

An API layer may be considered in future phases for carefully scoped use
cases.

**Cross-Domain Data Sharing**

Inter-domain or platform-level data sharing capabilities (such as
between business domains or environments) are not Sakura's
responsibility.

**Automatic RLS Remediation for Dataset Mismatches**

The system does not automatically resolve Row-Level Security mismatches
when reports using different datasets are added to an existing Audience.

- Initially:

  - **Audience A** → includes **Report A** → uses **Dataset A**

- Later:

  - App Owner **adds Report B** to **Audience A** → uses **Dataset B**

In this case, if Dataset A and Dataset B are using two different RLS
Security Models, Sakura does not manage or adjust these RLS policies.
Resolution of this type of conflict is out of scope of Sakura. Each
dataset may have distinct security rules, and combining them under one
Audience without harmonizing their security contexts can lead to
violations or unintended access. Responsibility for this integrity lies
entirely with the App Owner.

**OLS for Standalone Reports**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

Automatic management of Object-Level Security specific to non-App-based
reports (standalone reports) will not be managed by the system.

**Only RLS Permission**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

Requesting Row-Level Security (RLS) access for reports or applications
whose Object-Level Security (OLS) is not managed by Sakura is currently
out of scope.

In Sakura, an OLS request is a prerequisite for any RLS request. This
ensures that users first gain visibility to the reporting object (App or
Report) before any data-level filtering is applied.

This functionality may be considered in future phases once a broader
decoupling strategy is defined.

**Dimension-Based OLS Approver Workflows**

OLS permission approval processes based on dimensions like Reports,
Audiences, or similar will not be handled by Sakura. **While OLS
approvers can be defined at the SAR or Audience level, Sakura does not
support routing approvals based on specific security dimension values.**

For example, if Person A requests access to the **Finance Audience**
with an RLS combination of **US - CXM**, the request will be sent to the
**Finance Audience OLS approver**. There is **no mechanism to assign
different OLS approvers based on individual dimension values** like
\"US\" or \"CXM.\" The approver is fixed at the audience (or SAR) level
and does not vary depending on the RLS context of the request.

This means dimension-aware OLS routing logic is not supported.

**Out of Scope Items in Requirements Document**

All items, marked as Out of Scope in Requirements Document, are also out
of scope, unless otherwise explicitly stated in this document.

**Approval Tree Traversing Using Multiple Security Dimensions**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

Sakura does not currently support approver tree traversal based on
combinations of multiple security dimensions. Although individual
dimensions (such as Entity) can trigger hierarchical traversal to locate
an approver, **cross-dimensional traversal** is not implemented.

This limitation exists because there is no common set of dimensions that
applies uniformly across all security models in any workspace.
Supporting multiple dimension combinations would require **custom
traversal logic per Security Model**, which introduces significant
complexity.

At present, developing a dimension-aware traversal algorithm at Security
Model level is considered out of scope due to its technical and
operational complexity. However, this may be addressed in future
versions depending on demand and architectural evolution.

**In-App Notifications**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

The ability to display status updates and messages inside Sakura\'s user
interface under a dedicated **\"Notifications\"** section is currently
out of scope.

This functionality would include features such as:

- Read/Unread status tracking.

- Indication of the originating action (e.g., request approved,
  rejected, delegated)

- Notification messages summarizing the event.

It may be considered in a future version depending on user demand and
platform roadmap.

**Creating Requests for Multiple Users (On Behalf)**
![](media/image15.png){width="0.3951388888888889in" height="0.1375in"}

Unlike Sakura V1, in V2 it is no longer possible to submit access
requests on behalf of multiple users at once. Requests must be created
individually per user.

This limitation is necessary due to technical constraints in the
workflow:

- Each request involves validation steps for existing OLS and RLS
  access, which are highly user specific.

- Managing multiple users under one unified request would create
  complexity and ambiguity in how to handle different RLS/OLS contexts.

This restriction may be addressed in the future with the introduction of
a Shopping Cart-style experience.

**Smart Filters** ![](media/image15.png){width="0.3951388888888889in"
height="0.1375in"}

During the security dimension selections in the new request wizard, the
elements shown on current step could be filtered based on the selection
in previous steps. This is useful for scenarios like filtering
unnecessary Cost Centers based on the selected service line. However,
given the complexity of implementation and the tight timeline, this
feature may be considered in a future version depending on user demand
and platform roadmap.

# Assumptions, Risks, and Constraints

## Assumptions and Constraints

These are considered true for the scope of this project:

- Active Directory integration is available, queryable and up to date.

- Stakeholder requirements have been collected, finalized and signed
  off.

- Email notifications rely on the organization\'s SMTP server uptime.

- UI is desktop-optimized, and mobile accessibility is limited in the
  initial release.

- Users access the system within a secure network environment, such as
  via VPN or directly from the office.

- All Security Dimensions are sourced from relational SQL Systems
  accessible within dentsu network using SQL Server Authentication.

- Data required by Sakura is properly shaped and mapped to the Security
  Dimensions by responsible owners.

- Downstream Data sharing with external parties is handled via Views in
  Sakura Database. External Parties have the necessary tools to extract
  data from Sakura Database.

- Upstream systems providing source data are assumed to be stable and
  resilient, with any failures handled outside the scope of Sakura.

- Proper in-advance delegation planning (e.g., due to unavailability) is
  expected to be proactively managed by the admins (Workspace/App) to
  ensure functional continuity of Sakura.

- Stakeholders are aligned, well-coordinated, and have reached consensus
  on access management responsibilities, security requirements, and
  process ownership. In the event of disagreements or conflicts, they
  can collaborate constructively to reach resolution in a short time.

- All individuals configuring or using the Sakura are expected to act in
  good faith and in solidarity with the goals and responsibilities
  defined by the organization.

## Risks

These are the risks which might affect the Sakura process:

- SMTP email delivery failures may delay or prevent critical system
  notifications.

- Delegation and cleanup activities rely on manual actions, and it is
  expected that responsible individuals are aware of changes and perform
  them accurately and in a timely manner.

- Changes in Microsoft Power BI's platform could affect future system
  compatibility.

- Delays in access approvals may occur if Workspace and App
  Administrators do not actively monitor pending requests, as there is
  no automated SLA enforcement or escalation in Sakura. Responsibility
  for timely action lies with the respective administrators.

- VPN dependency may limit access flexibility for some users.

- Inconsistent understanding or interpretation of Security Dimensions
  may lead to incorrect access grants or denials.

- Lack of support for non-relational SQL sources limits data model
  flexibility.

- Absence of data mapping and shaping at source increases the risk of
  inconsistent integration.

- Poor data quality may propagate errors or create misleading access
  decisions and functionality of Sakura.

- Exclusion of external network sources and upstream system resiliency
  may result in failed requests or inaccessible datasets during outages.

- Possible delays and malfunction due to absence / unavailability of
  admins (Workspace / App) in access processing.

- Misalignment, lack of coordination, or absence of consensus among
  stakeholders may lead to delays, conflicting requirements, or unclear
  ownership in access and security processes.

- Misuse, negligence, or lack of cooperation by users or administrators
  may compromise the integrity of the system, leading to access issues,
  audit gaps, or weakened security enforcement.

# Appendix

## Glossary And Acronyms

| Term / Acronym | Description                                                                                                                         |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------|
| OLS            | Object Level Security -- Report/Audience visibility to users, simply it is about which reports can a user see                       |
| RLS            | Row Level Security -- Data visibility when someone opens a report, simply it is about which data is shown when user opens a report. |
| WS             | Workspace                                                                                                                           |
| WSA            | Workspace Admins (aka WS Owners & WS Tech Owners)                                                                                   |
| LM             | Line Manager                                                                                                                        |
| PBI            | Power BI                                                                                                                            |
| MS             | Microsoft                                                                                                                           |
| SAR            | Standalone Report                                                                                                                   |
| AUR            | Audience Report                                                                                                                     |
| WSO / WS Owner | Workspace Owner -- responsible for a Power BI and Sakura workspace.                                                                 |
| Approver       | A role that approves access based on assigned dimension (RLS), definitions (OLS + LM)                                               |
| DWH            | Datawarehouse                                                                                                                       |
| PC             | Profit Center                                                                                                                       |
| CC             | Cost Center                                                                                                                         |
| ORGA           | Organization                                                                                                                        |
| MSS            | Master Service Set                                                                                                                  |
| SL             | Service Line                                                                                                                        |
| PA             | Practice                                                                                                                            |
| CDI            | Client Data Insights                                                                                                                |
| FUM            | Finance Unified Model                                                                                                               |
| WFI            | Workforce Insights                                                                                                                  |
| AMER           | Americas                                                                                                                            |
| EMEA           | Europe, Middle East and Africa                                                                                                      |

## Diagrams

### Access Request Flow  {#access-request-flow}

![A black background with white rectangles AI-generated content may be
incorrect.](media/image16.png){width="6.3in"
height="1.5402777777777779in"}

Figure 13 - Access Request Wizard -- Using Report Catalog

![A black and white image of a black background AI-generated content may
be incorrect.](media/image17.png){width="6.3in"
height="1.1402777777777777in"}

Figure 14 - Access Request Wizard -- Advanced Mode

### Role-Functionality Matrix  {#role-functionality-matrix}

The table below outlines which functionalities in Sakura are available
to which user roles. This mapping is based on the descriptions provided
in section 2, where each role\'s capabilities are detailed. An "X"
indicates that the functionality is available to the respective role.

**Note**: Some roles such as Sakura Support are limited to read-only
access for visibility and troubleshooting purposes. Administrative
actions are reserved for Workspace Admins and Sakura Administrators.
Approvers act only within the approval workflow.

|                                                             | **Requester** | **Approver** | **Workspace Admin** | **Sakura Support** | **Sakura Administrator** |
|-------------------------------------------------------------|---------------|--------------|---------------------|--------------------|--------------------------|
| Request access for themselves                               | X             | X            | X                   | X                  | X                        |
| Request access for someone                                  | X             | X            | X                   | X                  | X                        |
| View existing access of themselves                          | X             | X            | X                   | X                  | X                        |
| View existing access of someone (within their context only) |               | X            | X                   | X                  | X                        |
| Viewing existing access of someone from all contexts        |               |              |                     | X                  | X                        |
| Approve Requests                                            |               | X            |                     |                    |                          |
| Append an Approver to an existing access request            |               |              | X                   |                    | X                        |
| Approve using email                                         |               | X            |                     |                    |                          |
| Revoke existing access                                      |               | X            | X                   |                    | X                        |
| Delegate                                                    |               | X            |                     |                    | X                        |
| Create or Delete a Workspace                                |               |              |                     |                    | X                        |
| View Workspaces                                             |               |              | X                   | X                  | X                        |
| Manage Apps & Audiences in WS                               |               |              | X                   |                    | X                        |
| Manage AUR in WS                                            |               |              | X                   |                    | X                        |
| Manage SAR in WS                                            |               |              | X                   |                    | X                        |
| Configure OLS Approvers                                     |               |              | X                   |                    | X                        |
| Administer Security Models                                  |               |              | X                   |                    |                          |
| Configure RLS Approvers                                     |               |              | X                   |                    |                          |
| Manage Security Dimensions                                  |               |              |                     |                    | X                        |
| View Emails Sent by Sakura                                  |               |              |                     | X                  | X                        |
| Export Requests to Excel                                    |               |              |                     |                    | X                        |
| Configure In-App Help                                       |               |              |                     |                    | X                        |
| Configure System Settings                                   |               |              |                     |                    | X                        |
| Manage Email Template                                       |               |              |                     |                    | X                        |

## Signoff References

Here is the list of signoff references for each Workspace by owners:

**DFI / FUM Workspace:**

\<Screenshot\>

**GDI Workspace:**

\<Screenshot\>

**CDI Workspace:**

\<Screenshot\>

**WFI Workspace:**

\<Screenshot\>

**AMER Workspace:**

\<Screenshot\>

**EMEA Workspace:**

\<Screenshot\>

[^1]: Check the Figure 2 - Report Delivery Scenarios. Report Delivery
    **Method I** covers the AUR and SAR is covered by **Method III** and
    **Method IV**.

[^2]: Visual Workflow could be found here: *[9.2.1 Access Request
    Flow]{.underline}*

[^3]: Unlike Sakura V1, in V2 it is no longer possible to submit access
    requests on behalf of multiple users at once. Requests must be
    created individually per single user. See Out of Scope Item:
    *[Creating Requests for Multiple Users (On Behalf)]{.underline}* for
    more info.

[^4]: These URL tags, how it should be or what it should be, aren't
    clear at the moment, it will be clarified during the development
    phase.

[^5]: For other limitations also check the Out-of-Scope part,
    "*[Approval Tree Traversing Using Multiple Security
    Dimensions]{.underline}*" section.

[^6]: Both RLS and OLS formats should be taken just as a sample and
    could be completely changed during the development phase, based on
    development priorities. Therefore, do not take these as final
    outcomes.

[^7]: Items with
    ![ROADMAP](media/image15.png){width="0.3284306649168854in"
    height="0.11604549431321085in"} (ROADMAP) symbol are planned for
    development in future versions of Sakura. While technically
    possible, they are not feasible to implement in the initial version
    due to time constraints. A roadmap outlining these plans will be
    shared separately.
