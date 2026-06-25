# Windows Server RDS Learning Path 67 — Patch an RDS Host without User Disruption

**Level:** Expert · **Module:** 67/70

## Goal
Patch Session Hosts sequentially while maintaining collection capacity and protecting user sessions.

## Setup
1. Measure current capacity and active/disconnected sessions.
2. Place one Session Host in drain mode.
3. Notify users and allow sessions to finish or reconnect elsewhere according to the maintenance plan.
4. Patch and restart the drained host.
5. Run service, app, profile, certificate and RDS health checks.
6. Return it to service.
7. Repeat on the second host.
8. Review user impact and events.

```powershell
Set-RDSessionHost -SessionHost RDSH02.corp.lab -NewConnectionAllowed No `
 -ConnectionBroker RDS01.corp.lab
Get-RDUserSession -ConnectionBroker RDS01.corp.lab
Get-HotFix | Sort-Object InstalledOn -Descending | Select-Object -First 20
Get-Service TermService,RDMS,Tssdis -ErrorAction SilentlyContinue
```

## Evidence
Store pre-patch sessions/capacity, drain state, patch list, restart timing, post-patch health, application tests and user-impact record under `evidence/`.

## Troubleshooting
Do not patch the second host until the first has passed health and functional tests. If capacity is insufficient, stop and reschedule.

## Security
Use approved updates and rollback plans. Preserve logs and verify security controls after restart.

## Rollback
Use the approved OS/application rollback method and keep the affected host drained until healthy.

## Next
`Windows-Server-RDS-Learning-Path-68-Configure-RDS-Drain-Mode`
