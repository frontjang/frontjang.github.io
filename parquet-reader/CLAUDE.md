# Claude Development Context

> **IMPORTANT**: This file should be maintained up-to-date with all debugging findings, verification results, and learnings from development work. Always update this file when discovering new patterns, issues, or solutions during testing and development.

> **Documentation Strategy**:
> - **Update CLAUDE.md** for important instructions, core patterns, and essential development context
> - **Update Reference Guides** when changes are made to detailed procedures or frameworks
> - **Create new instruction guides** for complex workflows that would bloat CLAUDE.md

> **Reference Guides**: Detailed guides available in `instruction/` folder - only read when needed:
> - `instruction/module-testing-guide.md` - Complete module testing framework guide

## Console Logging Strategy
**CRITICAL**: Remove all console.log statements from source code to save context size. Add logging **ONLY** during active debugging, then remove immediately.

**When to Add:**
- Active debugging - test failures or unexpected behavior
- Verification work - confirming new functionality works
- Troubleshooting - diagnosing selector or timing issues

**Guidelines:**
- Use prefixes: `🎯`, `📋`, `✅`, `❌`, `⚠️`
- Monitor with: `page.on('console', msg => console.log(msg.text()))`
- **ALWAYS REMOVE** after verification

**Test Types:**
- **Production**: `demo-scenarios-verification.spec.js` - Clean, no logs
- **Debug**: Temporary detailed logging for investigation

## Git Version Control Strategy
**MANDATORY**: Use git for all meaningful improvements with explicit version numbers (semantic versioning).

### Version Guidelines:
- **MAJOR (v2.0.0)**: Breaking changes, rewrites, API changes
- **MINOR (v1.2.0)**: New features, test suites, UI improvements, optimizations
- **PATCH (v1.1.1)**: Bug fixes, selector updates, documentation, cleanup

### Commit Message Format:
```
v1.2.3: Brief description

- What was changed
- Why it was changed
- Impact on functionality
- Testing verification performed
```

### Git Workflow:
1. Check version: `git tag --sort=-version:refname | head -1`
2. Make incremental progress
3. Ensure tests pass
4. Commit with version:
   ```bash
   git add .
   git commit -m "v1.2.3: Description..."
   git tag v1.2.3
   git push origin main --tags
   ```

**Don't Commit:** Temporary logs, incomplete work, experimental changes

## Deployment Strategy
**Two repositories**: Public static files vs. private source code.

### Repositories:
- **Public**: https://github.com/frontjang/frontjang.github.io/tree/master/parquet-reader (static files)
- **Private**: https://github.com/frontjang/react-parquet (source code)

### Commands:
```bash
# Deploy source code to private repo
./scripts/deploy-source.sh

# Build production files
npm run build

# Deploy static files to public repo
./scripts/deploy-pages.sh
```

### Security:
- Never deploy `src/` to public repo
- No sensitive info in `dist/`
- Maintain repo separation

## Demo Test Scenarios Framework
**Dual-purpose testing approach**: Demo scenarios serve both end users (showing functionality) and developers (automated verification).

### Purpose:
- **End Users**: Interactive demonstrations of all application features
- **Developers**: Comprehensive automated testing via Playwright
- **Documentation**: Live examples of how features work
- **QA**: Systematic verification of complete workflows

### Demo Scenarios Categories:
1. **Data Loading**: File upload, HTTP URLs, sample data
2. **Visualization**: Charts, graphs, interactive displays  
3. **Data Analysis**: Filtering, sorting, column management
4. **Data Export**: CSV, JSON, multiple format support
5. **Table Operations**: Column hide/show, reorder, resize
6. **Accessibility**: Keyboard shortcuts, screen reader support
7. **File Management**: Multiple files, tabs, drag & drop
8. **UI Customization**: Themes, dark mode, responsive design
9. **Integration Test**: End-to-end workflow verification

### Key Test Files:
- `tests/comprehensive-demo-verification.spec.js` - Complete scenario testing
- `tests/demo-scenarios-verification.spec.js` - Legacy production test
- `src/components/DemoMenu.js` - Interactive demo interface

### Demo Scenario Structure:
Each scenario includes:
```javascript
{
  id: 'unique-identifier',
  title: 'User-friendly name',
  description: 'Purpose and benefits',
  category: 'Functional group',
  difficulty: 'Easy|Medium|Hard',
  estimatedTime: 'Duration estimate',
  prerequisites: ['required-scenarios'],
  steps: ['Detailed step instructions'],
  userBenefit: 'What users learn',
  playwrightSelectors: {
    // Selector mappings for automation
  }
}
```

### Running Demo Tests:
```bash
# Comprehensive demo verification
npx playwright test tests/comprehensive-demo-verification.spec.js --headed

# Individual scenario testing
npx playwright test tests/comprehensive-demo-verification.spec.js --grep "Load Titanic"

# All demo tests
npx playwright test --grep "demo" 

# Debug mode
npx playwright test tests/comprehensive-demo-verification.spec.js --debug
```

### Critical Demo Selectors:
```javascript
// Demo menu access
'text="Demo Test Scenarios"'
'button:has-text("Run")'

// Core functionality
'[role="dialog"]'
'button:has-text("Done")'
'[role="grid"]' // Data table
'.MuiFab-root' // Floating action button

// Status verification
'.MuiSnackbar-root, .MuiAlert-root'
'[data-testid="row-count"]'
```

### Demo Testing Workflow:
1. **Prerequisites**: Ensure data loaded for dependent scenarios
2. **Execution**: Run scenario via DemoMenu or direct Playwright
3. **Verification**: Check UI state, notifications, data integrity
4. **Cleanup**: Reset state for next scenario
5. **Documentation**: Update guides with real results

### Benefits:
- **User Onboarding**: New users can explore features systematically
- **Feature Validation**: Every capability is tested and demonstrated
- **Documentation Accuracy**: Demos show actual current functionality
- **Regression Prevention**: Automated verification catches issues
- **Development Confidence**: Know features work as intended

## Living Documentation Framework
**Automated testing framework** that tests functionality and updates user guide with real, current information.

### Purpose:
- Tests real functionality with Playwright
- Captures actual screenshots  
- Updates UserGuideDialog with live results
- Maintains accuracy through automation

### Quick Start:
```bash
npm start
npm run update-docs  # (in another terminal)
npm run docs:quick   # headless
```

### Structure:
```
src/support/testing-framework/
├── DocumentationTester.js
├── ComponentUpdater.js
└── FeatureTestContext.js

src/support/tests/living-documentation.spec.js
src/support/scripts/update-docs.js
scripts/living-docs.config.js
```

### Configuration (`scripts/living-docs.config.js`):
```javascript
module.exports = {
  features: [
    {
      name: 'Data Loading Workflow',
      sampleData: 'src/support/sample_data/sales_data_sample.parquet'
    }
  ],
  documentation: {
    userGuideDialog: { enabled: true, autoUpdate: true }
  }
};
```

### Commands:
```bash
npm run update-docs                    # Full update
npm run update-docs:headless          # CI/CD
npm run docs:quick                    # Component only
npm run update-docs -- --verbose     # Debug
```

### Features:
- **Feature Testing**: Automated Playwright tests of real functionality
- **Component Updates**: Auto-updates UserGuideDialog with test results
- **Screenshot Management**: Auto-captures and manages screenshots in `public/guide-screenshots/`
- **Safety**: Automatic backups, validation, rollback capability
- **Integration**: CI/CD ready, pre-build hooks

### Benefits:
- Always accurate documentation
- Automated screenshot/content updates
- Data-driven real results
- Version control friendly

## Module Testing Framework
**Test module integrity and enable safe composition** - See `instruction/module-testing-guide.md` for complete guide.

### Commands:
```bash
npm run test:modules              # Test all modules
npm run test:modules:critical     # Critical modules only (stops on failure)
npm run test:modules:services     # Services only
```

### Key Concepts:
- **Module Integrity**: Each module tested in isolation with contracts
- **Safe Composition**: Verify modules can be combined safely
- **Performance Monitoring**: Runtime tracking of response times/errors
- **Priority Categories**: Critical → High → Medium → Low

### Before Building on Module:
```javascript
const result = await testRunner.testModule('hyparquetService');
if (result.status === 'passed') {
  // Safe to build on this module
}
```

## Environment Notes
**Windows Environment**: Running bash on Windows with PowerShell support.

### Development Server:
- **IMPORTANT**: `npm start` is already running before conversations begin
- Server available at http://localhost:3000
- **DO NOT** run `npm start` again - it will fail with EADDRINUSE error
- **DO NOT** start additional web servers
- App is ready for testing and development immediately

### UI Terminology:
- **Menu**: The left sidebar (formerly called "File Browser") containing navigation, file operations, data management, visualization, AI assistant, settings, and help sections
- The Menu can be toggled via the FAB (Floating Action Button) in the main interface

### File Operations:
- Use `powershell -Command "Move-Item 'source' 'destination'"` for moving files/folders
- Use `powershell -Command "Copy-Item 'source' 'destination' -Recurse"` for copying
- Use `powershell -Command "Remove-Item 'path' -Recurse -Force"` for deletion
- Standard Unix commands (`mv`, `cp`, `rm`) are not available

---

**Last Updated**: After module testing framework implementation and documentation consolidation  
**Test Coverage**: Demo scenarios, dialog handling, data loading, living documentation, module integrity  
**Status**: ✅ All systems verified, module testing framework operational