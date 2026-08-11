# ravn-profile-template

Template repository for Ravn reporter onboarding. Generate your own copy ("Use this
template", or let `scripts/get-ravn-profile.sh` from
[RavnSecurity/ravn-actions](https://github.com/RavnSecurity/ravn-actions) do it for you),
optionally add a read-only `RAVN_READONLY_TOKEN` secret, tweak `ravn.config.yml`, then run
the "Generate Ravn Profile" workflow from the Actions tab.

## The pin

`.github/workflows/generate-ravn-profile.yml` calls Ravn's collector by FULL COMMIT SHA:

```yaml
uses: RavnSecurity/ravn-actions/.github/workflows/collect-profile.yml@<sha>
```

That `@<sha>` is what lands in the signed provenance (the OIDC `job_workflow_ref` /
`job_workflow_sha` claims) and what verifiers check against Ravn's published trust list,
[`approved-workflow-shas.txt`](https://github.com/RavnSecurity/ravn-actions/blob/main/approved-workflow-shas.txt).
The pin here must always be a SHA on that list. The collector checks its own scripts out at
the same commit (`github.job_workflow_sha`), so the code that runs and the claim that gets
signed cannot diverge.

## Pending bump

ravn-actions' script-pin hardening
([ravn-actions#1](https://github.com/RavnSecurity/ravn-actions/pull/1), WS-B1 of
RavnSecurity/ravn-platform#137) is in flight. Once it merges, its merge-commit SHA gets
appended to the approved list and the pin above is bumped to that same SHA. The bump cannot
land before the merge — the SHA does not exist yet.
