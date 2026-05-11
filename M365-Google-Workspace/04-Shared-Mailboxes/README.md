# 🔍 Activity: Lab 2.4 — Shared Mailboxes & Distribution Lists

| Field | Value |
|---|---|
| **Environment** | Microsoft 365 Admin Center / Exchange Admin Center |
| **Ticket Type** | Service Request (P4 - Low) |
| **Status** | 🚧 In Progress |
| **Date** | 10 May 2026 |

---

## The Scenario
Bob Smith (Sales Director) has submitted a ticket. His team needs a central email address (`sales-enquiries@kdvlab.onmicrosoft.com`) that all his sales reps can read and reply from. He does not want to pay for a new Microsoft 365 license for this address.

## ITIL Alignment & The "Why"
This is a standard **Service Request** (fulfilling a pre-approved IT offering). 
In the Microsoft ecosystem, creating a dedicated user account just for a generic email address is a massive waste of money (licenses are expensive). Instead, we create a **Shared Mailbox**. Shared Mailboxes do not have passwords, cannot be logged into directly, and do not require a license. We then grant a Security Group (like `GRP_Sales`) permission to read it.

---

## Execution: Creating the Shared Mailbox

1. Log into the **[Microsoft 365 Admin Center](https://admin.microsoft.com)**.
2. In the left-hand menu, expand **Teams & groups** -> **Shared mailboxes**.
3. Click **+ Add a shared mailbox**.
4. Name it **Sales Enquiries** and set the email address to `sales-enquiries@kdvlab.onmicrosoft.com`.
5. Click **Save changes**.

## Execution: Assigning Permissions (Delegation)

Now that the mailbox exists, we need to allow the Sales team to use it.

1. Once the mailbox is created, click on it to open its properties.
2. Under the **Members** section, look for **Read and manage permissions** and click **Edit**.
3. Click **Add members**.
4. Search for your on-premises synced security group: **`GRP_Sales`**.
5. Select it and click **Save**.
6. Do the exact same thing for **Send as permissions** (so the team can reply *as* sales-enquiries, rather than from their personal emails).

> **Plain English:** Now, because `Bob Smith` is a member of `GRP_Sales` in your local Active Directory, this mailbox will magically appear at the bottom of his Outlook application within 60 minutes. If Bob leaves the company, you just remove him from `GRP_Sales`, and the mailbox disappears from his Outlook. No manual mailbox management required!

---

## 📝 Activity Verification

> **Action Required:** Take a screenshot of the "Read and manage permissions" window showing that `GRP_Sales` has been added, and save it to the `screenshots/` folder. We will embed it here!

---

## Related
- 🖥️ [Lab 2.1 - M365 Admin Centre Fundamentals](../01-M365-Admin-Centre/README.md)
