# Quick Reference: New CI/CD Workflow

## Your Release Checklist

### When you merge a feature to main

``` text
✅ Feature merged to main
   ↓
🌙 Nightly build automatically created
   (watch Actions tab, ~5 min)
   ↓
👥 Early testers download pre-release
   (marked as unstable/pre-release)
```

### When you're ready to release

``` text
✅ Work on main branch with conventional commits
   (feat: ... or fix: ... or chore: ...)
   
✅ release-please automatically creates version PR
   (checks every push to main)
   
✅ Review the PR:
   - Check changelog
   - Verify version bump
   - See installer test results in PR comments
   
✅ Add label "approved-for-release"
   (or just wait for auto-merge)
   
✅ Workflow auto-merges PR
   
✅ GitHub Release created (draft → published)
   
✅ winget-releaser publishes to Windows Package Manager
   (this happens automatically 5 min later)
```

---

## Workflow Files

| File | Trigger | Purpose | When to Check |
| ------ | --------- | --------- | -------------- |
| `test.yml` | Push/PR | Unit tests + code quality | On every code change |
| `nightly-build.yml` | Push to main (feat:/fix:) | Pre-release builds | After merging features |
| `build-release.yml` | Release-please PR + Release event | Installer building + testing | During release process |
| `release-approval.yml` | PR label: `approved-for-release` | Auto-merge + publish | When approving release |
| `release-please.yml` | Push to main | Version bump automation | Creates release PRs |
| `winget-releaser.yml` | Release published | Windows Package Manager | Auto, ~5 min after publish |

---

## Important Labels

Before first release, create this GitHub label:

**Name:** `approved-for-release`  
**Color:** #28a745 (green)  
**Usage:** Add to release-please PR to trigger auto-merge + auto-publish

---

## Typical Timeline

``` text
Monday 10:00 AM
├─ You finish feature
└─ git commit -m "feat: Add custom hotkeys"
   └─ git push

Monday 10:05 AM  
├─ Nightly build created
└─ v0.3.3-feat-nightly.20260103 ready to download
   (testers can try new feature)

Wednesday 2:00 PM
├─ After more testing, ready to release
├─ release-please PR created: "chore: release v0.3.3"
├─ Installer tests run on PR (✅ PASSED)
└─ You review changelog

Wednesday 2:15 PM
├─ You add label: "approved-for-release"
├─ Workflow auto-merges PR
├─ GitHub Release published
└─ winget-releaser triggers

Wednesday 2:20 PM
├─ Release v0.3.3 is LIVE
├─ Available on GitHub Releases
├─ Available on Windows Package Manager (pending ~5 min)
└─ 🎉 Done!
```

---

## Monitoring

### Check installer tests

1. Go to pull request created by `release-please[bot]`
2. Scroll to **Comments section**
3. Look for comment: "## Release Installers Built"
4. Check test status: ✅ PASSED or ❌ FAILED

### Check nightly builds  

1. Go to **Actions** tab
2. Filter by **nightly-build.yml**
3. Click on run → see artifacts

### Check releases

1. Go to **Releases** page
2. Look for `🌙 Nightly: ...` (pre-releases)
3. Look for `v0.x.x` (official releases)

---

## Emergency Override

If automated tests pass but you find an issue:

### Option 1: Fix and re-run

``` text
Fix the issue in a new commit
git push origin main
  → release-please creates a patch PR automatically
  → Tests run again
```

### Option 2: Emergency publish (not recommended)

``` text
GitHub web UI → Releases
Click "Edit" on draft release
Click "Publish release" manually
  (Skips approval workflow, goes straight to publish)
```

---

## Common Questions

**Q: Why skip tests on docs?**  
A: Documentation changes don't affect code, so testing wastes CI resources.

**Q: What if installer tests fail?**  
A: Check the PR comment for failure details. Common issues:

- Missing dependency (install InnoSetup manually)
- Build corruption (rebuild from source)
- App crash on launch (fix the code)

**Q: Can I manually build installers?**  
A: Yes! Use the scripts in `/scripts` folder locally:

```powershell
.\scripts\build_windows.ps1
.\scripts\compile_installer.ps1
```

**Q: When do nightly builds stay around?**  
A: 7 days (same as artifacts). After that, pre-release is still on GitHub but artifacts expire.

**Q: Can I skip nightly builds for certain commits?**  
A: Yes, just don't start with `feat:` or `fix:`. Use `chore:` or other prefixes.

---

## Support

For issues, check:

1. Actions tab → Workflow logs
2. PR comments → Installer test results  
3. Releases page → Pre-releases vs official

For questions, review: `CI_CD_REVAMP_GUIDE.md`
