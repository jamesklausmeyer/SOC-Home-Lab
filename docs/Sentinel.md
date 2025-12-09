# Overview
- These projects are explorations of core Sentinel features including EDR, threat intelligence integration, and email security.
- In the near future I plan to integrate my home AD domain to test features for hybrid environments

## Initial Configuration
- Deployed a new Log Analytics Workspace and enabled Microsoft Sentinel.
- Built a Windows Server VM and configured the supporting network infrastructure (VNet, subnet, and NSG).
- Enabled Defender for Cloud with Defender for Servers Plan 2, which includes endpoint protection via Microsoft Defender for Endpoint (MDE).
- Onboarded the VM into MDE using the official onboarding package from the Defender portal and validated sensor/EDR readiness.
- Confirmed successful onboarding by generating test detections.

## Testing Sentinel EDR
- Triggered an EICAR test malware event, which was immediately blocked and quarantined by MDE.
- Investigated the resulting incident in both Defender XDR and Sentinel:
  - Verified the alert details, device timeline, user context, and network activity.
  - Validated that no additional occurrences of the malicious file hash appeared elsewhere in the environment.
  - Reviewed RDP activity from the associated IP and confirmed that sign-in logs in Entra showed no indicators of compromise.
  - Closed the incidents as expected test activity.

## Threat Intelligence Integration
- Enabled Microsoft’s built-in threat intelligence connector and TAXII ingestion in Defender XDR.
- Connected the Microsoft Defender Threat Intelligence (MDTI) feed for automatically updated indicators.
- Created a custom threat intelligence watchlist containing suspicious IPs, file hashes, and domains.
- Verified ingestion by querying threat intelligence tables and confirming that both MDTI indicators and custom entries were available.
- Compared IPs from Sentinel security alerts against threat intelligence indicators to check for matches.
  - No overlaps were identified, confirming no alerts tied to known-malicious IPs from the TI sources.

## Email Security Testing
- Activated a Microsoft 365 E5 trial to access Defender for Office 365 features.
- Created two test users and validated that baseline protection policies (anti-phishing, anti-malware, anti-spam, Safe Links, Safe Attachments) were enabled.
- Since the default Safe Links policy could not be edited, I created a custom policy to enforce real-time scanning and click-tracking.
- Launched a phishing simulation using Defender’s built-in Attack Simulation Training.
- Both users received the simulated malicious email:
  - One user reported the message.
  - The other clicked the malicious link and was automatically assigned targeted security training related to phishing and business email compromise.
