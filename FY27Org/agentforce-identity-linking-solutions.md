# Agentforce Service Agent Identity Linking: Solution Options

**Customer:** Oxford  
**Created:** July 16, 2026  
**Classification:** Customer-Shareable Technical Documentation  
**Version:** 1.0

---

## Executive Summary

This document outlines three technical approaches to link Agentforce Service Agent interactions with verified user Contact records in your Experience Cloud messaging deployment. Each solution addresses the core requirement: **establishing reliable user identity on Messaging Sessions while preserving the Agentforce Service Agent's execution context as the Einstein Service Agent User.**

The solutions range from a cryptographically secure JWT-based approach (Option A) to simpler pre-chat collection methods (Options B and C), each with different tradeoffs in security, complexity, and record management.

---

## Technical Context

### The Challenge

When deploying an Agentforce Service Agent in Experience Cloud with messaging:

- **Without credential-based verification:** Messaging Sessions are not linked to user Contact records, limiting agent context and reporting capabilities.
- **With credential-based verification:** Messaging Sessions link correctly to Contacts, but the Agentforce Service Agent runs in the logged-in user's context instead of the Einstein Service Agent User's context, breaking data access permissions.

### Core Requirements

Any solution must:

1. **Preserve ASA execution context** — The agent must run as the Einstein Service Agent User to access order data and other restricted records
2. **Link sessions to Contacts** — Each Messaging Session must be associated with the correct user Contact for reporting and agent context
3. **Maintain security posture** — Identity verification must be reliable and appropriate for the use case
4. **Scale operationally** — The solution must work for large user populations without manual per-user configuration

---

## Solution Options

## Option A: Token-Based User Verification (JWT)

### Overview

Salesforce's Token-Based User Verification uses cryptographically signed JSON Web Tokens (JWTs) to verify end user identity at the messaging platform level. This approach creates a single, persistent, authenticated Messaging End User (MEU) record per user that is automatically linked to their Contact record.

**Official Documentation:** [Messaging for In-App and Web Token-Based User Verification](https://help.salesforce.com/s/articleView?language=en_US&id=service.miaw_token_based_user_verification_setup.htm&type=5)

### How It Works

1. **Key Generation & Registration**  
   Generate RSA key pairs and register the public key as a JSON Web Keyset in Salesforce Setup (Messaging for In-App and Web User Verification).

2. **Channel Configuration**  
   Attach the keyset to the Messaging channel in Messaging Settings and enable User Verification.

3. **Server-Side JWT Generation**  
   Build a secure server-side mechanism (Apex REST endpoint or Connected App with JWT flow) that generates signed JWTs for authenticated Experience Cloud users. The user's unique identifier is included as the `sub` claim in the JWT.

4. **Client-Side Token Passing**  
   Deploy a custom Lightning Web Component on the Experience Cloud site that:
   - Calls the Apex endpoint to retrieve the JWT
   - Passes it to the messaging client via `embeddedservice_bootstrap.userVerificationAPI.setIdentityToken()`

5. **Salesforce Validation**  
   Salesforce validates the JWT server-side using the registered public key. A single, persistent, authenticated MEU record is created (or reused) with a platform key in the format: `v2/iamessage/AUTH/<config>/uid:<sub>`

### Key Benefits

✅ **Single MEU per user** — Eliminates record proliferation entirely. Each user gets one MEU regardless of how many sessions they initiate.

✅ **Server-side identity verification** — The JWT is cryptographically signed; identity cannot be spoofed from the browser's DevTools.

✅ **ASA execution context preserved** — User Verification does not enable user-based credentialing. The ASA continues running as the Einstein Service Agent User.

✅ **Clean outbound messaging** — One MEU per user means the recipient picker shows a single, unambiguous record.

✅ **Automatic Contact linkage** — The verified MEU's Contact can be populated via the Omni-Channel flow or through the standard MEU-Contact relationship.

✅ **Strongest security posture** — Cryptographic verification provides the highest level of identity assurance.

### Risks & Considerations

⚠️ **LWR site compatibility** — Salesforce documentation confirms Aura site support as of Spring '24. LWR support should be validated in your sandbox before committing to this approach.

⚠️ **Implementation complexity** — Requires RSA key management, a server-side JWT generation endpoint, and LWC development. This is the most technically complex option.

⚠️ **Certificate lifecycle** — RSA keys have expiration dates and must be rotated. A key rotation process should be established.

### Implementation Requirements

| Component | Effort | Notes |
|-----------|--------|-------|
| RSA key generation | Low | One-time setup in Salesforce Setup |
| Apex JWT endpoint | Medium | Requires secure Apex REST service |
| LWC development | Medium | Custom component to call endpoint and pass token |
| Testing & validation | Medium | Validate LWR compatibility, MEU creation, ASA context |

### Recommended For

Organizations requiring the strongest security posture, single MEU per user, and clean outbound messaging with the resources to implement and maintain a JWT infrastructure.

---

## Option B: Pre-Chat Email Collection with Contact Matching

### Overview

A pre-chat form collects the user's email address before the messaging session begins. The email is prepopulated from the Experience Cloud session so the user sees it already filled in (minimal friction). The Omni-Channel flow matches the email to a Contact record and populates a custom Contact lookup field on the Messaging Session.

### How It Works

1. **Pre-Chat Form Configuration**  
   Create a pre-chat form field for email address on the Embedded Service Deployment.

2. **Email Prepopulation**  
   Use a custom Lightning Web Component on the Experience Cloud site to prepopulate the email field from the authenticated user's session. The user sees their email already filled in and clicks to proceed.

3. **Custom Field Creation**  
   Create a custom Contact lookup field on the Messaging Session object (e.g., `User_Contact__c`).

4. **Omni-Channel Flow Matching**  
   In the Omni-Channel flow:
   - Receive the email value from the pre-chat form
   - Use a Get Records element to match it to a Contact record
   - Update the Messaging Session with the matched Contact ID

### Key Benefits

✅ **Simple implementation** — Uses standard pre-chat configuration, a single LWC, and straightforward flow logic.

✅ **ASA execution context preserved** — No credentialing changes required.

✅ **Contact attribution achieved** — The custom lookup on the Messaging Session provides a clean reference for reporting, case linking, and agent context.

✅ **Familiar pattern** — Similar to how many orgs handle identity in MIAW today.

✅ **Quick to deploy** — Can be implemented rapidly to unblock pilot or production rollout.

### Risks & Considerations

⚠️ **Guest MEU proliferation continues** — Each session still creates a new guest MEU. The outbound messaging clutter problem is not solved by this approach alone.  
**Workaround:** Agents use the custom Contact lookup on the Messaging Session rather than the MEU for identification.

⚠️ **Client-side data trust** — The email is prepopulated from the browser. A technically sophisticated user could modify it via DevTools. In practice, the risk is low for authenticated users on a credentialed site, but it is not cryptographically verified.

⚠️ **Email matching assumptions** — The flow assumes a 1:1 match between email and Contact. If multiple Contacts share an email, additional matching logic is needed.

⚠️ **Minor UX friction** — Even with prepopulation, the user sees a pre-chat form with their email before starting the conversation. This adds one click compared to a seamless no-form experience.

### Implementation Requirements

| Component | Effort | Notes |
|-----------|--------|-------|
| Custom field on Messaging Session | Low | Standard Salesforce field creation |
| Pre-chat form configuration | Low | Standard Embedded Service config |
| Email prepopulation LWC | Low-Medium | Lightweight component to read user email |
| Omni-Channel flow updates | Medium | Add Get Records + Update Records elements |
| Email matching logic | Low-Medium | Defensive logic for edge cases (multiple matches, no match) |

### Recommended For

Organizations seeking a pragmatic, quick-to-deploy solution that achieves Contact attribution on Messaging Sessions with minimal implementation complexity. Best as an immediate solution or fallback if Option A is not feasible.

---

## Option C: Hidden Pre-Chat Field with User ID Passthrough

### Overview

A hidden pre-chat field silently passes the logged-in user's Salesforce User ID from the Experience Cloud site to the Omni-Channel flow. The flow queries the User record to get the `ContactId` and updates the guest MEU's Contact lookup. This is the simplest and fastest option to implement.

### How It Works

1. **Hidden Pre-Chat Field**  
   Create a hidden pre-chat form field on the Embedded Service Deployment.

2. **User ID Injection**  
   Deploy a custom Lightning Web Component (or JavaScript in site head markup) that:
   - Retrieves the current user's Salesforce User ID from the Experience Cloud session
   - Populates the hidden pre-chat field with the User ID

3. **Omni-Channel Flow Lookup**  
   In the Omni-Channel flow:
   - Receive the User ID value from the pre-chat form
   - Use a Get Records element to query the User record
   - Extract the `ContactId` from the User record
   - Update the guest MEU with the matched Contact ID

### Key Benefits

✅ **Simplest implementation** — Uses standard pre-chat configuration and straightforward flow logic.

✅ **ASA execution context preserved** — No credentialing changes required.

✅ **No visible UX friction** — Hidden field means the user doesn't see a pre-chat form (in most configurations).

✅ **Direct User-to-Contact mapping** — Leverages the built-in User.ContactId relationship.

✅ **Fastest to deploy** — Can be implemented in under a week with minimal development effort.

### Risks & Considerations

⚠️ **Guest MEU proliferation continues** — Each session still creates a new guest MEU. Same record clutter as Option B, but Contact identity is still captured for reporting.

### Implementation Requirements

| Component | Effort | Notes |
|-----------|--------|-------|
| Hidden pre-chat field | Low | Standard Embedded Service config |
| User ID injection component | Low | Simple LWC or JavaScript |
| Omni-Channel flow updates | Low | Add Get Records element to query User |

### Recommended For

Organizations seeking the fastest, simplest path to Contact attribution on Messaging Sessions. Ideal when speed of deployment is the top priority and you're working within an authenticated Experience Cloud environment.

---

## Comparison Matrix

| Criteria | **A: JWT Verification** | **B: Pre-Chat Email** | **C: Hidden User ID** |
|----------|-------------------------|------------------------|------------------------|
| **Single MEU per user** | ✅ Yes | ❌ No | ❌ No |
| **ASA runs as Einstein Agent User** | ✅ Yes | ✅ Yes | ✅ Yes |
| **Contact on Messaging Session** | ✅ Automatic | ✅ Custom field | ✅ Via MEU update |
| **Clean outbound messaging** | ✅ Yes | ❌ No | ❌ No |
| **Implementation complexity** | High | Low | **Lowest** |
| **Visible UX friction** | None | Minor (pre-chat form) | **None** |
| **Time to deploy** | 2-3 weeks | 1 week | **< 1 week** |

---

## Recommendation

### For Fast Implementation: Option C — Hidden User ID Passthrough

If your priority is getting Contact attribution working quickly with minimal development effort, **Option C is the recommended starting point**. It provides:

- Fastest time to value (< 1 week implementation)
- Simplest technical approach
- No visible UX friction for users
- Direct User-to-Contact mapping leveraging existing relationships

This approach gets Contact identity linked to Messaging Sessions immediately and can serve as either a permanent solution or a tactical first step while evaluating more complex options.

### For Long-Term Architecture: Option A — Token-Based User Verification

If you need to solve MEU proliferation and want the cleanest long-term architecture, **Option A provides the most complete solution**:

1. The ASA runs as the Einstein Service Agent User
2. Each user gets a single persistent MEU
3. Identity is verified server-side with cryptographic signing
4. Outbound messaging is clean and unambiguous

**Next step:** Validate this approach in your sandbox environment to confirm LWR site compatibility.

### Middle Ground: Option B — Pre-Chat Email Collection

Option B balances simplicity with a visible confirmation step. It's a good choice if you want slightly stronger identity validation than Option C without the complexity of Option A.

**These approaches are not mutually exclusive.** You can implement Option C immediately to unblock your deployment, then evaluate whether Option A's benefits justify the additional implementation effort for your long-term architecture.

---

## Implementation Considerations

### For Option A (Token-Based User Verification)

**Certificate Management**  
Establish a process for RSA key rotation before the initial certificate expires. Document the keyset configuration for your operations team.

**Apex Endpoint Security**  
The JWT generation endpoint should only be accessible to authenticated Experience Cloud users. Apply appropriate Apex class-level security and test that unauthenticated requests are rejected.

**LWR Component Deployment**  
The LWC that calls the Apex endpoint and passes the JWT must be added to the site header or footer to ensure it is present on every page where messaging is available.

**Testing Validation**  
Validate in sandbox that:
- The JWT is accepted and a verified MEU is created
- The same MEU is reused across multiple sessions for the same user
- The ASA still runs as the Einstein Service Agent User (test by verifying data access in flow actions)

### For Option B (Pre-Chat Email)

**Custom Field**  
Create `user_Contact__c` (Lookup to Contact) on the Messaging Session object. Add to page layouts and any relevant reports or dashboards.

**Email Matching**  
Implement defensive logic in the Omni-Channel flow for edge cases:
- Multiple Contacts share the same email
- No Contact matches the provided email
- Log unmatched cases for review and troubleshooting

**Prepopulation**  
Build a lightweight LWC that retrieves the current user's email and populates the pre-chat field. Test that prepopulation works reliably on your LWR site (note: some community reports indicate intermittent null values with similar approaches).

**Outbound Messaging Workaround**  
Train agents to use the custom Contact field on the Messaging Session (or the Contact record's phone/email directly) for outbound messaging, rather than the MEU-based recipient picker.

### For Option C (Hidden User ID)

**Component Deployment**  
Deploy the User ID injection component (LWC or JavaScript) to your Experience Cloud site header or footer to ensure it's present on all pages where messaging is available.

**Omni-Channel Flow**  
Add a Get Records element to query the User object by Id, then extract the ContactId field and update the MEU's Contact lookup field.

---

## Next Steps

1. **Decision:** Review the three options with your development team to determine the preferred approach based on timeline and architecture goals.

2. **Quick Implementation (Option C):** For fastest time to value, implement the hidden User ID approach to establish Contact attribution immediately.

3. **Long-Term Evaluation (Option A):** If MEU management and clean outbound messaging are priorities, validate JWT-based verification in your sandbox to confirm LWR site compatibility.

4. **Implementation Support:** Once the approach is selected, we can provide detailed implementation guides, sample code, and testing scripts.

---

**Questions or need technical guidance?** Reach out to your Salesforce Solutions Engineer for implementation support and sandbox validation assistance.
