# ✅ CI/CD Pipeline Revamp - Deployment Checklist

## Implementation Status: COMPLETE ✅

All workflows have been updated and documented. Follow this checklist to deploy.

---

## Pre-Deployment Checklist

### Code Review

- [ ] Review all modified workflow files:
  - [ ] `.github/workflows/test.yml` - doc skip optimization
  - [ ] `.github/workflows/release-pipeline.yml` - installer testing added
  - [ ] `.github/workflows/nightly-build.yml` - ⭐ NEW
  - [ ] `.github/workflows/release-approval.yml` - ⭐ NEW

### Documentation Review

- [ ] Read `CI_CD_REVAMP_GUIDE.md` - full overview
- [ ] Read `CICD_QUICK_REFERENCE.md` - quick reference
- [ ] Read `IMPLEMENTATION_SUMMARY.md` - what changed
- [ ] Read `SETUP_GITHUB_LABEL.md` - label setup instructions

### Team Communication (if applicable)

- [ ] Share documentation with team
- [ ] Explain new release workflow
- [ ] Clarify what changed and why

---

## Deployment Steps

### Step 1: Commit to Git

```bash
cd c:\Users\conve\GitHub\OverKeys

# Stage all changes
git add .github/workflows/
git add *.md

# Verify changes
git status

# Commit with clear message
git commit -m "ci: revamp cicd with nightly builds, installer testing, and approval gates

- Add doc-only skip to test workflow (faster feedback)
- Add hybrid installer testing: integrity + smoke test
- Add nightly pre-release builds on feat/fix commits
- Add approval gate for release publishing
- Add comprehensive documentation and guides"

# Push to main
git push origin main
```

### Step 2: Create GitHub Label

1. Go to: <https://github.com/conventoangelo/OverKeys/settings/labels>
2. Click "New label"
3. Fill in:
   - **Name:** `approved-for-release`
   - **Color:** #28a745 (or any color)
   - **Description:** Approved for automatic merge and release publication
4. Click "Create label"

### Step 3: Verify Workflows

1. Go to Actions tab: <https://github.com/conventoangelo/OverKeys/actions>
2. Look for recent workflow runs
3. Verify no errors (green checkmarks ✅)
4. Test runs should show:
   - `test.yml` - runs on push
   - Installer test comments on any release PRs

---

## Post-Deployment Testing

### Test 1: Doc-Only Changes (should skip tests)

```bash
# Create a test branch
git checkout -b test/doc-skip

# Edit a markdown file
echo "# Test" >> README.md
git add README.md
git commit -m "docs: test readme update"
git push origin test/doc-skip

# Create PR to main and check:
# ✅ test.yml should NOT run (doc-only)
# Delete branch after test
git checkout main
git branch -d test/doc-skip
```

**Expected Result:** Actions tab shows NO test workflow run

### Test 2: Nightly Build Trigger

```bash
# Create a test branch
git checkout -b test/nightly

# Commit with feat: prefix
echo "test" >> test.txt
git add test.txt
git commit -m "feat: add test feature"
git push origin test/nightly

# Check Actions → nightly-build.yml
# Should create pre-release with version like:
# feat-nightly.20260102-abc1234

# Delete branch after test
git checkout main
git branch -d test/nightly
```

**Expected Result:** Pre-release created on Releases page marked 🌙 Nightly

### Test 3: Release Approval Flow

```bash
# Push a regular commit to main (triggers release-please)
git commit -m "feat: test release feature" --allow-empty
git push origin main

# Wait ~1 minute for release-please to create PR
# Go to Pull Requests tab
# Look for PR: "chore: release v0.x.x"
# Check:
# ✅ Installer tests run on PR
# ✅ PR comment shows test results
# ✅ Add label "approved-for-release"
# ✅ Workflow auto-merges PR
# ✅ Release created and published
# ✅ winget-releaser triggered ~5 min later
```

**Expected Result:** Automated release workflow completes

---

## Rollback Plan (if needed)

If you need to revert to the old workflow:

```bash
# Find the commit hash of this implementation
git log --oneline | head -5

# Revert the specific commit
git revert <commit-hash> --no-edit

# Push the revert
git push origin main

# Or: Delete new workflows manually
git rm .github/workflows/nightly-build.yml
git rm .github/workflows/release-approval.yml
git commit -m "ci: remove new nightly and approval workflows"
git push origin main
```

---

## Post-Deployment Validation

### Verify Changes Landed

- [ ] Check: <https://github.com/conventoangelo/OverKeys/tree/main/.github/workflows>
- [ ] Confirm all 6 workflow files exist
- [ ] Confirm new files: nightly-build.yml, release-approval.yml
- [ ] Confirm edits to: test.yml, release-pipeline.yml

### Verify Label Created

- [ ] Check: <https://github.com/conventoangelo/OverKeys/labels>
- [ ] Confirm `approved-for-release` label exists
- [ ] Label color is visible (green or chosen color)

### Monitor First Release

When creating first release post-deployment:

- [ ] Watch Actions tab for all workflow runs
- [ ] Verify test.yml runs on any code changes
- [ ] Verify release-pipeline.yml installer tests pass
- [ ] Verify release-approval.yml instructions appear
- [ ] Test label approval triggers auto-merge/publish
- [ ] Confirm winget-releaser runs after publish

---

## Success Criteria

You'll know deployment is successful when:

✅ All workflows appear in Actions tab  
✅ Test workflow skips on doc-only PRs  
✅ Nightly builds create pre-releases on feat/fix commits  
✅ Release-please PR shows installer test results  
✅ Adding `approved-for-release` label triggers auto-publish  
✅ No errors in any workflow runs

---

## Common Post-Deployment Issues

### Issue 1: Nightly build not triggering

**Solution:** Verify commit message starts with `feat:` or `fix:`

```bash
git log --oneline -1
# Should show: feat: ... or fix: ...
```

### Issue 2: Installer tests timeout

**Solution:** Increase timeout in release-pipeline.yml

- Change `timeout-minutes: 30` to `timeout-minutes: 45`

### Issue 3: Release-approval workflow doesn't trigger

**Solution:** Ensure label name is exactly `approved-for-release` (no typos)

### Issue 4: Auto-merge fails on release PR

**Solution:** Check for merge conflicts or required status checks

- Ensure PR is mergeable before adding label

---

## Documentation Files Created

For reference, these documents were created:

| File                        | Purpose                                     |
| --------------------------- | ------------------------------------------- |
| `CI_CD_REVAMP_GUIDE.md`     | Complete implementation guide with diagrams |
| `CICD_QUICK_REFERENCE.md`   | One-page quick reference for daily use      |
| `IMPLEMENTATION_SUMMARY.md` | Technical details of what changed           |
| `SETUP_GITHUB_LABEL.md`     | Instructions for creating GitHub label      |
| `DEPLOYMENT_CHECKLIST.md`   | This file - deployment steps                |

---

## Next Steps After Successful Deployment

1. **Communicate** with your team about new release process
2. **Train** team on using `approved-for-release` label
3. **Archive** old release notes/documentation
4. **Plan** for future enhancements:
   - Signed installers (code signing)
   - macOS/Linux builds (if applicable)
   - Slack/email notifications on releases
   - Dependency update automation (Dependabot)

---

## Support

If you encounter issues:

1. **Check** Actions tab workflow logs
2. **Review** CI_CD_REVAMP_GUIDE.md troubleshooting section
3. **Inspect** GitHub PR comments for test results
4. **Read** workflow YAML files for configuration details

---

**Status:** ✅ Ready for Deployment  
**Date:** January 2, 2026  
**Estimated Deployment Time:** 15 minutes (commit + label creation + testing)

Good luck with your new CI/CD pipeline! 🚀
