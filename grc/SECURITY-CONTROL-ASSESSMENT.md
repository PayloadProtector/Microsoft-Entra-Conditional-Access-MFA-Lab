# GRC Security Control Assessment — Conditional Access and MFA

**Assessment type:** Portfolio lab / simulated internal control review  
**Environment:** Microsoft Entra ID test tenant  
**Scope:** Conditional Access MFA, geographic restrictions, legacy authentication, and Temporary Access Pass (TAP)  
**Evidence source:** [Existing lab README](../README.md) and its linked screenshots  
**Assessment limitation:** Review of documented lab evidence; not an independent production audit.

## 1. Risk and control objectives

| Risk | Control objective | Implemented lab control |
|---|---|---|
| Compromised passwords enable unauthorized access | Require stronger authentication for scoped users | `CA-Require-MFA-Test-Users` (On) |
| Sign-ins from disallowed geographic locations | Evaluate location-based access restrictions | `CA-Block-NonUS-Test-Users` (Report-only) |
| Legacy authentication weakens MFA protections | Identify and evaluate blocking legacy client sign-ins | `CA-Block-Legacy-Authentication` (Report-only) |
| Insecure enrollment or recovery | Provide controlled bootstrap authentication | Temporary Access Pass and Microsoft Authenticator configuration |

## 2. Testing and available evidence

| Control | Documented test or evidence | What the evidence supports | Remaining verification |
|---|---|---|---|
| MFA Conditional Access | Policy screenshot and Conditional Access What If screenshots | MFA policy is configured **On**; simulated conditions show policy applicability | Inspect a representative sign-in log's Conditional Access tab and authentication details to confirm MFA was required and satisfied for an in-scope sign-in |
| Geographic restriction | Policy state and What If simulation | Intended block behavior was evaluated | Not proof of enforcement: policy remained **Report-only** |
| Legacy authentication restriction | Client-app configuration and policy state | Block rule configured for evaluation | Not proof of enforcement: policy remained **Report-only**; test with representative client-app logs if available |
| TAP onboarding | TAP configuration, test sign-in, authentication log | Successful TAP sign-in documented; log records TAP and success | Does not alone demonstrate the MFA Conditional Access policy enforced MFA |

## 3. Preliminary findings

**GRC-001 — Geographic restriction not enforced in this lab.**  
**Status:** Observed configuration state; not necessarily a deficiency in a test tenant.  
**Evidence:** Geographic policy remained Report-only.  
**Risk if deployed unchanged in production:** Disallowed-location sign-ins would not be blocked by this policy.  
**Recommendation:** Review exclusions, test impact, establish emergency access safeguards, and obtain approval before enabling enforcement.

**GRC-002 — Legacy authentication block not enforced in this lab.**  
**Status:** Observed configuration state; not necessarily a deficiency in a test tenant.  
**Evidence:** Legacy-authentication policy remained Report-only.  
**Risk if deployed unchanged in production:** Targeted legacy client sign-ins would not be blocked by this policy.  
**Recommendation:** Review sign-in telemetry and application dependencies, test in Report-only, then plan controlled enforcement.

**GRC-003 — MFA enforcement evidence requires additional validation.**  
**Status:** Evidence gap, **not** a confirmed control failure.  
**Evidence:** MFA policy listed On; What If simulation; separate successful TAP authentication event.  
**Recommendation:** Add a redacted sign-in log showing the MFA policy's actual evaluation and authentication requirement for an in-scope sign-in.

## 4. Assessment conclusion

The documented lab demonstrates configured identity controls, policy simulation, and successful TAP authentication. The MFA policy was **On**, while geographic and legacy-authentication policies were **Report-only**. The current documented evidence does **not** establish that every proposed control was enforced in a live sign-in. A production effectiveness conclusion would require additional representative sign-in records, control scope review, and operating-period evidence.

**Portfolio note:** This is a training assessment based on a lab, not a formal compliance certification or an audit of a real organization.
