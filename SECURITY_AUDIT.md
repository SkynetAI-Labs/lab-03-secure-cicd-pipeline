# Repository Security Audit — Lab 03

**Review date:** 24 October 2026  
**Reviewed source commit:** [`c757196ee71ee49c668dc03c4e932d535b5ab8a8`](https://github.com/SkynetAI-Labs/lab-03-secure-cicd-pipeline/tree/c757196ee71ee49c668dc03c4e932d535b5ab8a8)  
**Scope:** Public repository configuration, documentation, retained evidence and available workflow results. This is a repository audit, not a fresh live-environment assessment or penetration test.

## Outcome
Reviewed main-branch CI and the separate Gitleaks workflow passed for `c757196ee71ee49c668dc03c4e932d535b5ab8a8` on **24 October 2026**. The existing supply-chain audit and its limitations remain applicable.

## Evidence checked
- [CI run 40055131810](https://github.com/SkynetAI-Labs/lab-03-secure-cicd-pipeline/actions/runs/40055131810): tests, Bandit, container scan and gated publishing succeeded.
- [Gitleaks run 40055131825](https://github.com/SkynetAI-Labs/lab-03-secure-cicd-pipeline/actions/runs/40055131825): successful separate secrets check.
- Direct actions use full commit SHAs; the base image uses a digest; Gitleaks archive checksum verification is configured.
- The scanned image is transferred to publishing without rebuilding. A CycloneDX SBOM artifact was uploaded.
- Publishing produced image digest `sha256:b3b31308855b45a6a640a5d921853edc0b2c35fa7ed4619251a04bf00ddbdf6d` and [attestation 59321382](https://github.com/SkynetAI-Labs/lab-03-secure-cicd-pipeline/attestations/59321382).
- The README's independently verified attestation refers to the earlier digest beginning `d7254f21`. It does not independently verify the newer image above.
- Repository files cannot independently prove current branch-protection or administrator-bypass settings.

## Recommendations
1. **Priority 1 before consuming a new image — Independently verify its exact registry digest and attestation**, enforcing repository/workflow identity.
2. **Priority 2 — Recheck current branch protection in GitHub settings**, including required checks, administrator enforcement and the desired approving-review count. The documented configuration requires zero approving reviews.
3. **Priority 2 — Consider including secrets scanning in the direct publishing dependency chain.** Gitleaks currently runs separately; publishing directly waits on tests, Bandit and container scanning.
4. **Priority 2 — Add reviewed automated dependency updates**, full SBOM schema validation and hash-locked Python scanner dependencies.
5. **Priority 3 — Preserve evidence beyond expiring Actions artifacts** when long-term audit retention is required.

No live application deployment is demonstrated. Attestations establish identity and signed workflow claims, not application safety or a particular SLSA assurance level.

## Banner and evidence integrity

The README links an existing `skynet-lab-03.png` banner and the asset is present. No replacement was needed in this portfolio-wide pass. Historical evidence was preserved. Recommendations above remain open unless explicitly described as completed.

## References

- [GitHub Actions security guidance](https://docs.github.com/en/actions/reference/security/secure-use)
- [AWS S3 transport-encryption guidance](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingEncryptionInTransit.html)
- [Kubernetes NetworkPolicy semantics](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

