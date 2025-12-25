# Issue: Windows Role Cmdlets Missing

## OSI Phase
Phase 06 – Presentation (Blocked)

## Symptoms
- Install-WindowsFeature not recognized
- New-WebBinding not recognized
- WebAdministration module missing

## Initial Hypotheses
- Running Windows 10/11 instead of Server
- Required modules/features not installed

## Evidence
- Install-WindowsFeature Web-Server → command not found
- Import-Module WebAdministration → module not found
- Get-WindowsOptionalFeature → lists available features

## Root Cause
System was running Windows 10/11, which lacks Server role cmdlets and modules.

## Remediation
- Used Windows Optional Features to enable IIS
- Installed WebAdministration module

## Validation
- Import-Module WebAdministration → succeeded
- New-WebBinding → command available

## Lessons Learned
- Confirm OS version before running server role commands
- Document required modules for lab automation
