# Customer platform handover assessment

## Outcome

The managed project is a production customer-facing commerce platform with
storefront, seller, administration, logistics, and mobile surfaces. It is
already deployed through a mature GitHub Actions and AWS path. ApexYard should
govern planning, review, QA, and release evidence around that path rather than
replace it.

## Harnessability summary

| Dimension | Assessment | Evidence / next action |
|---|---|---|
| Source of truth | Strong | `CLAUDE.md` documents live routes, generated artifacts, deployment, and recovery. |
| Local development | Strong | Docker Compose, restore data, and reset scripts are documented. |
| Verification | Moderate | PHP linting, static analysis, tests, and deployment smoke checks exist; add explicit acceptance-criteria evidence to each PR. |
| Delivery safety | Strong | GitHub Actions deploys to AWS and includes smoke-test / rollback behavior; leave this pipeline unchanged. |
| Agent guidance | Improved | `AGENTS.md` adds a concise operating manual and reinforces the production boundary. |

## Recommended operating loop

1. Capture work as a GitHub issue with acceptance criteria.
2. Record an architecture or migration decision when the change affects data,
   infrastructure, security, or a cross-cutting pattern.
3. Implement one focused branch and pull request.
4. Run targeted checks plus the repository test/static-analysis checks.
5. Review security and customer-facing behavior as applicable.
6. Obtain QA evidence against the acceptance criteria.
7. Merge through the existing repository policy. The existing production
   GitHub Actions workflow remains the only deployment channel.

## Known risks

- Homepage, product pages, JavaScript, CSS, and icon fonts have generated or
  minified live artifacts; edits must follow the paired-file instructions.
- Production-only configuration, uploads, caches, and service-account files
  must remain outside Git and deployment payloads.
- Customer data and infrastructure changes need an explicit rollback or
  fix-forward plan before implementation.

## Follow-up backlog

- Add a standard PR checklist for acceptance criteria, verification evidence,
  rollback, and deployment-boundary confirmation.
- Establish a small set of repeatable customer-facing smoke checks for the
  storefront, seller, admin, logistics, and mobile surfaces.
- Add architecture diagrams after the first deeper review of the live routes
  and AWS boundaries.
