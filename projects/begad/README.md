# Customer platform

**Status**: active  
**Tier**: P0

## What it is

This is a UAE-focused commerce platform with a customer storefront, seller
tools, administration, logistics workflows, and mobile drivers.

## Tech stack

- PHP 8.3 custom application served by Apache and PHP-FPM
- MariaDB 10.5
- AWS, CloudFront, RDS, EFS, and GitHub Actions
- Docker-based local development

## Operating notes

- Production changes are deployed from `main` through GitHub Actions.
- The application repo already contains detailed production recovery and
  deployment guidance in `CLAUDE.md`; ApexYard governs the delivery process
  around that source of truth.
- The local checkout is intentionally not duplicated under `workspace/` until
  a deeper ApexYard review requires it.
