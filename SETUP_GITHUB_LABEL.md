## Setup: Create GitHub Label for Release Approval

The new release approval workflow requires one GitHub label to function. Follow these steps:

### Step-by-Step Instructions

#### 1. Go to GitHub Repository Settings
- Navigate to: https://github.com/conventoangelo/OverKeys/settings/labels
- Or: Repo → Settings → Labels

#### 2. Click "New label"

#### 3. Fill in the Details
- **Label name:** `approved-for-release`
- **Description:** Approved for automatic merge and release publication
- **Color:** #28a745 (green) or any color you prefer

#### 4. Click "Create label"

#### 5. Verify
You should now see `approved-for-release` in your labels list.

---

### Using the Label

When you want to publish a release:

1. release-please creates a PR: "chore: release v0.3.3"
2. Review the PR
3. Go to the PR and click **"Labels"** on the right side
4. Select **`approved-for-release`**
5. Workflow automatically:
   - Merges the PR
   - Publishes the release
   - Triggers winget publishing

---

### Optional: Add Label to Multiple Places

#### Add to PR Templates
If you have a PR template, you can suggest this label in the release-please PR by adding to the template:
```markdown
- [ ] Mark as approved-for-release when ready to publish
```

#### Add to Release Instructions
You can also pin instructions in a GitHub discussion or repository wiki.

---

### Troubleshooting

**Q: Label doesn't show in release-approval workflow?**  
A: The workflow looks for exact name `approved-for-release`. Make sure there are no typos.

**Q: Workflow still doesn't trigger after adding label?**  
A: Give it 10-30 seconds. GitHub Actions can be slow to detect label additions.

---

That's it! The new CI/CD pipeline is now ready to use.
