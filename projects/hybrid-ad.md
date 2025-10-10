# Hybrid AD + Azure AD Connect

## Overview
Deploying hybrid identity synchronization between on-prem Active Directory and Azure AD using Password Hash Sync, Intune enrollment, and Conditional Access.

![Diagram](../assets/images/hybrid-ad-diagram.png)

## Key Components
- Windows Server 2022 domain controller  
- Azure AD Connect (Password Hash Sync)  
- Intune Autopilot with dynamic device groups  

## Highlights
- Automated domain join and MDM enrollment  
- Conditional Access enforcing MFA and compliant devices  
- Group Policy modernization using cloud-based baselines  

## Lessons Learned
- Keep AADC staging servers updated to prevent schema drift.  
- Document OU and filter rules early — they get complex fast.
