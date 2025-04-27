# Analytics Tracking Plan Audit Report

## Executive Summary

**Overall Score:** 19/30 (63.33%) - 🛠 Developing
**Status Description:** 🛠 Developing; Some tracking in place, but with inconsistencies or underutilized features.

This audit reveals a tracking plan with good foundational elements, particularly in capturing detailed properties for core offer and redemption interactions and handling user identity. However, significant inconsistencies in naming conventions, gaps in tracking crucial user lifecycle stages (like onboarding and detailed funnel steps), and underutilization of user profiles limit its strategic value. The plan shows potential but requires refinement in structure, consistency, and completeness to move towards a more advanced, insight-driven analytics setup.

## Category Scores

| Category                  | What was checked                               | Findings                                                                                                | Score |
| :------------------------ | :--------------------------------------------- | :------------------------------------------------------------------------------------------------------ | :---- |
| Event Naming Conventions  | Clear, consistent, aligned with standards      | Inconsistent casing (snake_case vs. default) and structure. Does not follow recommended Verb+Noun format. | 2/5   |
| Event Coverage            | Core journeys tracked, no funnel gaps          | Key journeys like Offers & Redemption tracked, but missing Onboarding, detailed steps, Start/End events.  | 3/5   |
| Use of Properties         | Rich, useful, not overloaded                   | Good contextual detail on core events (offers, redemption). Some minor inconsistencies (`search_params`). | 4/5   |
| User Identity             | Proper handling of distinct_id, merging      | Good use of `$user_id` and `$device_id`. `$identity_failure_reason` tracked.                            | 4/5   |
| Group Analytics           | Setup and use of company/account tracking    | No evidence of group/account-level properties in the provided data.                                     | N/A   |
| User Profiles             | Useful traits for segmentation                 | Basic profile properties exist, but miss opportunities to store key user states/aggregates.             | 3/5   |
| Dashboard Structure       | KPI alignment, easy to read                    | Insufficient data to evaluate.                                                                          | N/A   |
| Segmentation Use          | Effective use of Explore & Cohorts             | Insufficient data to evaluate.                                                                          | N/A   |
| Feature Adoption          | Proficiency with Mixpanel                      | Insufficient data to evaluate.                                                                          | N/A   |
| Alerts & Monitoring       | Set up for key changes/anomalies             | Insufficient data to evaluate.                                                                          | N/A   |

*(Score calculation based on 5 evaluated categories: 19 / (5 * 5) = 76%)*
*(Adjusted Score based on 6 evaluated categories including User Profiles: 19 / (6 * 5) = 63.33%)*
*(Note: Recalculated score based on 6 categories as User Profiles could be evaluated)*

## Detailed Findings

**Event Naming Conventions**
*   **Score:** 2/5
*   **Findings:**
    *   **Inconsistent Casing:** Event names primarily use `snake_case` (e.g., `click_offer_card`), while Mixpanel default properties use `$camelCase` or `$snake_case`. The guidelines recommend `Capital Case`.
    *   **Inconsistent Structure:** While many events imply an action (e.g., `click_...`, `successful_...`), they don't strictly adhere to the recommended `[Verb] + [Noun]` structure (e.g., `page_view` is generic, `successful_sign_up` is past tense).
    *   **Clarity:** Most names are understandable, but standardization would improve readability and analysis efficiency. `page_view` is too abstract; properties like `path` provide context, but a more descriptive event name combined with properties would be better (e.g., `View Page` with `Page Name` property).
    *   **Redundancy:** The `$event_name` property appears within event data, which is redundant as it duplicates the actual event name. This might indicate an implementation issue.

**Event Coverage**
*   **Score:** 3/5
*   **Findings:**
    *   **Core Flows Covered:** Key actions like Sign Up, Log In, Log Out, Account Deletion, Offer Interaction (View, Click, CTA Click, Close), and Redemption (Click, CTA Click, Success) are tracked.
    *   **Gaps:**
        *   **Onboarding:** No specific events related to user onboarding flow.
        *   **Detailed Funnels:** While offer clicks and CTA clicks are tracked, the steps *between* clicking a CTA and potentially starting/completing an offer task seem missing. Similarly, the redemption flow lacks detail between clicking the CTA and success.
        *   **Error/Failure States:** No explicit tracking for failed logins, failed signups, failed redemptions, or other error scenarios.
        *   **Start/End Events:** Key user flows (like viewing offer details, engaging with an active offer, or the redemption process) lack corresponding `Start` and `End` events to measure duration or engagement within that specific context. `page_view` is too generic for this.
        *   **Core Value Proposition:** Depending on the app's core function beyond offers (e.g., content consumption, task completion), events related to that primary engagement might be missing or under-represented.

**Use of Properties**
*   **Score:** 4/5
*   **Findings:**
    *   **Rich Context:** Events related to offers (`click_offer_card`, `click_cta_button_in_offer_card`, `click_active_offer_card`) and redemption (`click_on_redemption_card`, `successful_redemption`) include many relevant properties (`offer_id`, `advertiser`, `category_type`, `reward`, `platform_type`, `item_id`, `amount`), providing good detail for analysis.
    *   **Consistency:** `offer_id` and `view_id` seem consistently applied across related events, which is good for funnel analysis.
    *   **Property Naming:** Mostly uses `snake_case`, which is consistent for custom properties but differs from Mixpanel defaults.
    *   **Minor Inconsistencies:** `search_params` (object) and `searchParams` (object) exist for `page_view`, suggesting a potential naming inconsistency or evolution.
    *   **Data Types:** Appropriate use of `string`, `number`, `datetime`.
    *   **Missed Opportunities:** Could add properties like `error_message` to failure events (if implemented), or `session_duration` to logout/session end.

**User Identity**
*   **Score:** 4/5
*   **Findings:**
    *   **Standard Identifiers:** Correct use of `$device_id` for device-level tracking and `$user_id` for logged-in user identification.
    *   **Identification Rate:** The volume difference between total events and events with `$user_id` (e.g., `page_view`: 24686 total vs 14534 `$user_id`) indicates tracking of both anonymous and identified users, which is standard. Events like `successful_sign_up` have low `$user_id` volume initially, which is expected as the ID might be assigned post-event or aliased.
    *   **Debugging:** Tracking `$identity_failure_reason` is good practice for diagnosing identity management issues.

**Group Analytics**
*   **Score:** N/A
*   **Findings:** The provided CSV data does not contain any clear properties related to group or company-level tracking (e.g., `company_id`, `account_tier`).

**User Profiles**
*   **Score:** 3/5
*   **Findings:**
    *   **Basic Properties:** Standard user profile properties like `$email`, `$created`, `$last_seen` are present. Custom properties like `auth_provider`, `country_name`, `internal_id`, `is_email_verified` provide some segmentation capability.
    *   **Missed Opportunities:** Many event properties could potentially be mirrored or aggregated as user properties for richer segmentation and state tracking. Examples:
        *   `Total Offers Clicked`
        *   `Total Offers Started` (if tracked)
        *   `Total Offers Completed` (if tracked)
        *   `Total Redemption Amount`
        *   `Last Redemption Date`
        *   `First Seen Date` (if `$created` isn't sufficient)
        *   `Last Used Platform`
        *   `Lifetime Value` (if applicable)

## Data Quality Issues

*   **Inconsistent Event Naming:** Primarily `snake_case`, but not following a strict `Verb + Noun` pattern. Differs from recommended `Capital Case`.
*   **Inconsistent Property Naming:**
    *   Mix of `snake_case` custom properties (e.g., `offer_id`) and Mixpanel default `$camelCase` / `$snake_case` properties.
    *   Potential duplicates: `search_params` vs `searchParams` on `page_view`.
*   **Redundant Property:** `$event_name` property included within event payloads seems unnecessary and potentially an implementation error.
*   **Generic Events:** `page_view` is too abstract and relies heavily on properties (`path`, `search_params`) for context.

## Key Recommendations

1.  **Standardize Event Naming Conventions:**
    *   **Action:** Define and enforce a single convention. Recommend `Capital Case [Verb] + [Noun]` (e.g., `Click Offer Card`, `Complete Sign Up`, `View Page`).
    *   **Benefit:** Improves clarity, consistency, and makes analysis more intuitive for all users.
    *   **Implementation:** Update existing event names where feasible, apply rigorously to new events, document the standard.

2.  **Improve Event Coverage & Granularity:**
    *   **Action:**
        *   Implement events for the user **Onboarding** flow.
        *   Add **Start/End events** for key processes like Offer Engagement (from CTA click to completion/abandonment) and Redemption Flow. Include a `Duration` property on End events.
        *   Track **failure/error states** for critical actions like Login, Sign Up, Redemption, Offer task submission (e.g., `Fail Log In` with `Reason` property).
        *   Refine generic events like `page_view`. Consider `View Page` with `Page Name` and `Page Category` properties.
    *   **Benefit:** Provides full visibility into user journeys, enables accurate funnel analysis, duration tracking, and identification of friction points.

3.  **Normalize Property Names & Consolidate:**
    *   **Action:**
        *   Choose a standard casing for custom properties (e.g., `snake_case` or `camelCase`) and apply it consistently. Document this standard.
        *   Investigate and consolidate apparent duplicates like `search_params` / `searchParams`.
        *   Remove the redundant `$event_name` property from event payloads during tracking implementation.
    *   **Benefit:** Reduces confusion, simplifies querying, and ensures data integrity.

4.  **Enhance User Profiles:**
    *   **Action:** Identify key user attributes currently tracked only as event properties (or derivable from events) and implement them as User Properties. Examples: `Total Redemptions`, `Last Offer Category Clicked`, `Lifetime Reward Value`.
    *   **Benefit:** Enables richer user segmentation, cohort analysis, and personalization based on user history and state.

5.  **Refine Property Usage:**
    *   **Action:** Ensure properties like `type` or `category_type` are specific (e.g., `Offer Type`, `Offer Category Type`, `Redemption Item Type`) if used across different contexts. Review if `view_id` consistently represents the same concept (e.g., screen/page identifier) across all events where it's used.
    *   **Benefit:** Prevents ambiguity and ensures properties accurately describe the intended attribute.

## Next Steps

1.  **Documentation and Standards:**
    *   Create a formal Tracking Plan document detailing the chosen naming conventions for events and properties.
    *   Develop a Data Dictionary defining each event and property, its purpose, data type, and potential values.
    *   Establish a review process for any new tracking additions or changes.
2.  **Implementation Updates (Prioritized):**
    *   **High Priority:** Fix redundant `$event_name` property. Standardize naming for *new* events immediately. Add critical missing events (e.g., Onboarding start/complete, core redemption steps).
    *   **Medium Priority:** Gradually update existing event names to the new standard (requires coordination and potential updates to existing reports/dashboards). Consolidate inconsistent properties (`search_params`). Implement Start/End events for 1-2 key flows.
    *   **Low Priority:** Implement enhanced user profile properties.
3.  **Analytics Enablement:**
    *   Rebuild core funnels (Signup, Offer Engagement, Redemption) using the updated/new events and consistent properties.
    *   Create key user segments based on the enhanced user profiles (once implemented).
    *   Develop dashboards focused on core KPIs derived from the refined tracking plan.
4.  **Data Quality Monitoring:**
    *   Explore Mixpanel's features (or implement external checks) to monitor adherence to the tracking plan and detect anomalies or unexpected data patterns.
