# Microsoft Entra ID Conditional Access & MFA Security Lab

## Overview

This hands-on identity and access management lab demonstrates the implementation and validation of Microsoft Entra ID Conditional Access and modern authentication controls.

The project simulates an enterprise identity-security environment where Conditional Access policies are used to require multifactor authentication (MFA), evaluate geographic access restrictions, restrict legacy authentication, configure Microsoft Authenticator, provision Temporary Access Pass (TAP), and investigate authentication activity through Microsoft Entra sign-in logs.

The lab emphasizes a **Zero Trust approach**: access is evaluated based on identity, authentication method, location, client type, and policy conditions rather than automatically trusting a successful username and password.

---

## Project Objectives

- Configure Conditional Access policies for test identities.
- Require MFA for designated users.
- Design a geographic restriction for sign-ins outside the United States.
- Design a policy to block legacy authentication clients.
- Use Report-only mode to evaluate security controls before enforcement.
- Test Conditional Access behavior with the What If tool.
- Configure Microsoft Authenticator security settings.
- Provision a Temporary Access Pass for secure user onboarding.
- Validate authentication activity using Microsoft Entra sign-in logs.

---

## Technologies & Skills

**Platform**
- Microsoft Entra ID
- Microsoft Azure

**Identity & Access Management**
- Conditional Access
- Multifactor Authentication (MFA)
- Microsoft Authenticator
- Temporary Access Pass (TAP)
- Authentication Methods
- Named Locations

**Security Operations**
- Sign-in Log Analysis
- Conditional Access What If
- Report-only Policy Testing
- Authentication Troubleshooting
- Zero Trust Access Controls

---

## Conditional Access Architecture

Three Conditional Access policies were configured for the lab environment.

| Policy | Purpose | Final Lab State |
|---|---|---|
| `CA-Require-MFA-Test-Users` | Require multifactor authentication for scoped test users | **On** |
| `CA-Block-NonUS-Test-Users` | Evaluate blocking access outside the approved U.S. location | **Report-only** |
| `CA-Block-Legacy-Authentication` | Evaluate blocking legacy authentication clients | **Report-only** |

The geographic and legacy authentication policies were intentionally retained in **Report-only** mode during testing. This allowed their expected impact to be evaluated before enforcement.

### Final Conditional Access Configuration

![Final Conditional Access Policies](screenshots/CA%20Policies%202026-08-28%203.46.39%20PM.png)

---

## 1. Conditional Access What If Testing

Before fully enforcing access controls, I used the Microsoft Entra **Conditional Access What If** tool to simulate authentication conditions.

This allowed me to determine which policies would apply to a test sign-in without depending on a real-world blocked authentication attempt.

### What If Configuration

![Conditional Access What If](screenshots/what%20if%202026-08-27%203.42.01%20PM.png)

The resulting evaluation showed the applicable Conditional Access controls, including:

- Require multifactor authentication
- Block access based on the geographic policy conditions
- Block access based on legacy authentication conditions

### Policy Evaluation Results

![Conditional Access What If Results](screenshots/What%20If%20results%202026-08-27%203.48.08%20PM.png)

This testing approach helped validate policy logic before moving security controls into full enforcement.

---

## 2. Legacy Authentication Protection

Legacy authentication protocols can create additional identity-security risk because older authentication methods may not support modern controls such as MFA.

I configured Conditional Access client-app conditions to evaluate legacy authentication traffic.

The policy was designed to apply a **Block access** control when the targeted legacy authentication conditions were met.

### Client App Configuration

![Legacy Authentication Client Apps](screenshots/client%20app%202026-08-27%203.45.14%20PM.png)

The policy remained in **Report-only** mode during the lab so its behavior could be evaluated safely before enforcement.

---

## 3. Authentication Method Policies

I reviewed and configured tenant authentication-method policies to support modern authentication.

The environment included Microsoft Authenticator and Temporary Access Pass as available authentication methods.

![Authentication Method Policies](screenshots/Auth%20method%20portal%202026-08-27%205.24.43%20PM.png)

This provided the authentication foundation for the MFA and onboarding portions of the project.

---

## 4. Microsoft Authenticator Configuration

Microsoft Authenticator was configured as an authentication method for the lab environment.

![Microsoft Authenticator](screenshots/Ms%20Auth%202026-08-27%205.27.25%20PM.png)

I also reviewed Microsoft Authenticator security settings associated with authentication notifications, including number matching and contextual information presented during authentication.

### Authenticator Security Settings

![Microsoft Authenticator Security Configuration](screenshots/config%20auth%20002%202026-08-27%205.34.34%20PM.png)

These controls strengthen MFA by giving users additional context when approving authentication requests.

---

## 5. Temporary Access Pass

I provisioned a **Temporary Access Pass (TAP)** for the test user as part of the authentication onboarding workflow.

TAP provides a temporary credential that can be used to bootstrap authentication and help users establish supported authentication methods.

### TAP Provisioning

![Temporary Access Pass Configuration](screenshots/add%20TAP%202026-08-28%201.51.28%20PM.png)

---

## 6. User Authentication with TAP

After provisioning the Temporary Access Pass, I tested the authentication workflow using the test identity.

The Microsoft sign-in process requested the Temporary Access Pass rather than relying exclusively on the user's standard password.

![John Miller TAP Authentication](screenshots/John%20miller%20TAP%202026-08-28%202.03.29%20PM.png)

This demonstrated the user-facing portion of the TAP authentication workflow.

---

## 7. Authentication Verification & Sign-In Log Investigation

Configuration alone does not prove that an authentication workflow worked.

After performing the TAP authentication, I investigated the Microsoft Entra authentication details to verify the event.

The sign-in data recorded:

- **Authentication method:** Temporary Access Pass
- **Authentication result:** Succeeded
- **Succeeded:** `true`

![Temporary Access Pass Sign-In Verification](screenshots/TAP%20log%202026-08-28%203.29.55%20PM.png)

This provided evidence that the Temporary Access Pass authentication workflow was successfully executed and recorded by Microsoft Entra ID.

---

## Security Workflow

```text
User Sign-In
     |
     v
Microsoft Entra ID
     |
     v
Conditional Access Evaluation
     |
     +-------------------+
     |                   |
     v                   v
Identity / MFA       Sign-In Context
                         |
                  +------+------+
                  |             |
                  v             v
               Location     Client Type
                  |             |
                  +------+------+
                         |
                         v
                  Policy Decision
                         |
              +----------+----------+
              |                     |
              v                     v
         Require MFA           Block Access
              |               when applicable
              v
      Authentication Method
              |
        +-----+------+
        |            |
        v            v
  Authenticator     TAP
        |            |
        +-----+------+
              |
              v
        Access Decision
              |
              v
         Sign-In Logs
              |
              v
       Admin Verification
```

---

## Key Security Takeaways

This project reinforced several important identity-security concepts:

1. **Conditional Access should be tested before broad enforcement.** What If and Report-only mode provide methods for evaluating policy impact.

2. **MFA adds protection beyond passwords.** A compromised password alone should not automatically provide access to protected resources.

3. **Authentication context matters.** Location, client type, authentication method, user identity, and targeted resources can influence an access decision.

4. **Legacy authentication should be addressed.** Older authentication mechanisms can bypass protections available through modern authentication.

5. **Logs are essential for validation.** Microsoft Entra sign-in and authentication details provide evidence of how an authentication attempt was processed.

6. **Temporary Access Pass supports secure onboarding.** TAP can provide a controlled bootstrap authentication method for users establishing authentication credentials.

---

## Skills Demonstrated

- Microsoft Entra ID Administration
- Conditional Access
- Multifactor Authentication
- Microsoft Authenticator
- Temporary Access Pass
- Authentication Method Management
- Conditional Access What If Analysis
- Report-only Policy Validation
- Legacy Authentication Controls
- Location-Based Access Controls
- Sign-In Log Investigation
- Identity Security Troubleshooting
- Zero Trust Principles

---

## Project Outcome

The completed lab demonstrates a layered Microsoft Entra identity-security workflow covering **policy configuration, authentication controls, policy simulation, user authentication, and log-based verification**.

Rather than treating Conditional Access as a configuration-only exercise, the project validates security behavior through What If analysis and authentication logs while distinguishing between controls actively enforced and controls evaluated in Report-only mode.

---

## Disclaimer

This project was completed in a controlled lab environment using test identities and resources. Report-only policies were used where appropriate to evaluate Conditional Access behavior without representing those controls as actively enforced production restrictions.
