# Deploy to GitHub - prebuy-ai Organization

## Repository Setup Instructions

Since I don't have direct access to push to GitHub, here are the steps to get this repository up to the prebuy-ai organization:

### Option 1: Create Repository via GitHub Web Interface

1. **Go to GitHub**: Navigate to https://github.com/organizations/prebuy-ai/repositories/new
2. **Repository Details**:
   - **Repository name**: `theme-integration-snippet` or `shopify-theme-snippet`
   - **Description**: "Universal Shopify theme integration snippet for Prebuy selling plans with Markets support"
   - **Visibility**: Public (or Private if preferred)
   - **Initialize**: Don't initialize with README, .gitignore, or license (we already have them)

3. **After creation**, you'll get a repository URL like: `https://github.com/prebuy-ai/theme-integration-snippet.git`

### Option 2: Use GitHub CLI (if available)

```bash
# Navigate to the repository directory
cd /Users/alisterhewitt/Documents/DEV/Agents/PREBUY-THEME-SNIPPET

# Create repository in prebuy-ai organization
gh repo create prebuy-ai/theme-integration-snippet --public --description "Universal Shopify theme integration snippet for Prebuy selling plans with Markets support"
```

### Push to GitHub

Once the remote repository is created, run these commands:

```bash
# Navigate to repository directory
cd /Users/alisterhewitt/Documents/DEV/Agents/PREBUY-THEME-SNIPPET

# Add remote origin (replace with actual repository URL)
git remote add origin https://github.com/prebuy-ai/theme-integration-snippet.git

# Push to GitHub
git push -u origin main
```

### Repository Features to Enable

After pushing, consider enabling these GitHub features:

1. **Issues**: Enable for merchant support and feedback
2. **Wiki**: For extended documentation
3. **Discussions**: For community questions
4. **Pages**: To create a documentation website
5. **Releases**: For version management

### Suggested Repository Settings

- **Branch Protection**: Protect the `main` branch
- **Auto-merge**: Enable for approved PRs
- **Delete head branches**: Auto-delete after merge
- **Topics**: Add relevant topics like `shopify`, `liquid`, `theme`, `integration`, `prebuy`

## Current Repository Status

✅ **Ready for GitHub**:
- [x] Git repository initialized
- [x] All files committed
- [x] .gitignore configured
- [x] .gitattributes set up
- [x] LICENSE file added (MIT)
- [x] Comprehensive README.md
- [x] Detailed installation guide
- [x] Example integrations included

## Files Ready for Push

```
PREBUY-THEME-SNIPPET/
├── .git/                           # Git repository
├── .gitignore                      # Git ignore rules
├── .gitattributes                  # Git attributes
├── LICENSE                         # MIT License
├── README.md                       # Main documentation
├── INSTALLATION.md                 # Detailed installation guide
├── QUICK-REFERENCE.md              # Quick reference card
├── sync-notion.md                  # Notion sync instructions
├── prebuy-selling-plan.liquid      # Main integration snippet
└── examples/
    ├── buy-buttons-integration.liquid     # Buy buttons example
    └── product-card-integration.liquid    # Product card example
```

## Next Steps

1. Create the repository on GitHub (Option 1 or 2 above)
2. Add the remote origin
3. Push the code
4. Configure repository settings
5. Update the Notion document with the GitHub repository link

Let me know the final repository URL once it's created, and I can update the documentation to reference it!
