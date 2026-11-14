# okta-oig-migration

Documentation of a real initiative to move a company's Okta SSO applications into Okta Identity Governance (OIG), replacing informal access requests with a governed, auditable access-request workflow.

## Problem

As the SaaS footprint grows, informal "ask IT in Slack" access requests stop scaling and stop being auditable. The goal was to move every Okta-integrated app into OIG so access could be requested, approved, and reviewed through a consistent, logged process instead.

## Approach

Apps were migrated into OIG in waves, grouped by provisioning complexity. A separate "birthright" tier was defined for apps every employee should automatically receive at onboarding, versus apps that should always go through an access request.

## What Was Migrated

- **OIG-governed apps:** Adobe, Amazon Business, Claude, Figma, GitHub Enterprise Cloud, Hex, monday.com, Snowflake, Tableau Cloud
  - Adobe is the fullest example of automated fulfillment: the Okta Workflows connector built for Adobe Creative Cloud provisioning is live in production and wired directly to OIG, so an approved access request triggers automatic license/role provisioning with no manual step afterward
- **Birthright apps (auto-issued at onboarding):** Notion, a password manager, security awareness training, and a baseline device management enrollment
- **In progress at time of writing:** AWS IAM Identity Center (owned by another team member), a contract-management platform, and a deployment platform's access-group model

## Decisions and Trade-offs Worth Documenting

- Chose to keep a generative AI tool on its existing plan tier rather than upgrade to an enterprise tier solely to gain SCIM support, after weighing the cost against the governance benefit
- For one analytics platform, kept a birthright department-level group rule for baseline access, with granular project/team-level access still routed through an OIG access request after onboarding, rather than trying to model the entire permission tree as birthright
- Built in a recurring check for apps where the deactivation push silently stops working, after finding and fixing several apps where offboarded users were not actually being deprovisioned downstream

## Tech Stack

- Okta Identity Governance (OIG)
- Okta group rules and group push
- SCIM 2.0 (where supported)

## Notes

All company names, group names, and app-specific configuration details have been generalized or removed. This repo documents the governance approach and decision-making, not exported OIG policy configuration.

---

**Security note:** Never commit real credentials, API keys, tenant IDs, internal URLs, or any company-identifying data. Replace with placeholders before publishing, and check commit history too.
