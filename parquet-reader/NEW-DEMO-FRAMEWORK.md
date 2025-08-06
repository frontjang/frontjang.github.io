# Demo Test Framework v2.0 - JSON-based Test Scenarios

## Overview

The new Demo Test Framework provides a comprehensive, JSON-based approach to defining, executing, and monitoring test scenarios. This framework separates concerns between test definition, execution simulation, and monitoring.

## Architecture

```
JSON Test Definitions → Interactive HTML → JavaScript Simulation → Playwright Monitoring
     (src/test-scenarios/)    (demo-scenarios.html)    (Browser JavaScript)    (scenario-monitor.spec.js)
```

## Key Components

### 1. JSON Test Scenario Definitions (`src/test-scenarios/`)

Each test scenario is defined as a JSON file with:

- **Pre-conditions**: What must be true before the test can run
- **Post-conditions**: What must be true for the test to pass  
- **Steps**: Detailed test steps with expected outcomes
- **Metadata**: Category, difficulty, estimated time, dependencies

Example structure:
```json
{
  "id": "load-titanic-dataset",  
  "title": "Load Titanic Dataset",
  "order": "1.1",
  "preConditions": [
    {
      "condition": "Application is loaded and ready",
      "check": "document.readyState === 'complete'"
    }
  ],
  "postConditions": [
    {
      "condition": "Data is loaded with 891 rows and 12 columns",
      "check": "window.appState?.isDataLoaded && window.appState?.currentData?.length === 891",
      "message": "PASS: Load Titanic Dataset - Data loaded: 891 rows, 12 columns"
    }
  ],
  "steps": [
    {
      "step": 1,
      "action": "Click 'Load HTTP URL' button in the toolbar",
      "selector": "button:has-text('Load HTTP URL')",
      "expected": "HTTP URL dialog opens"
    }
  ]
}
```

### 2. Test Scenario Processor (`scripts/process-test-scenarios.js`)

Processes JSON files and generates interactive HTML:

```bash
npm run test-scenarios                    # Process and generate HTML
npm run test-scenarios:build             # Build mode with optimizations
```

Features:
- Loads and validates JSON scenarios
- Generates interactive HTML with embedded JavaScript
- Creates scenario execution simulator
- Provides export functionality

### 3. Interactive HTML Demo (`demo-scenarios.html`)

Generated HTML file containing:

- **Visual scenario browser** with expandable details
- **JavaScript simulation engine** that mimics user interactions
- **Pre/post condition validation** engine
- **Progress monitoring** and logging
- **Results export** functionality

Key features:
- ✅ Run individual scenarios or all scenarios
- ✅ Visual progress tracking and logging
- ✅ Pre/post condition validation
- ✅ Results export to JSON
- ✅ App state simulation and tracking

### 4. Playwright Monitor (`tests/scenario-monitor.spec.js`)

Playwright test that monitors JavaScript execution:

```bash
npm run test-scenarios:monitor           # Monitor with Playwright
```

The monitor:
- ❌ **Does NOT execute scenarios itself**
- ✅ **Watches JavaScript execution** in the browser
- ✅ **Reports success/failure** based on JavaScript results
- ✅ **Validates scenario structure** and metadata
- ✅ **Tests export functionality**

## Current Test Scenarios

### 1.1 Load Titanic Dataset
- **Category**: Data Loading
- **Pre-conditions**: Application ready, no data loaded
- **Post-conditions**: 891 rows, 12 columns loaded
- **Steps**: 8 detailed steps from URL loading to verification

### 2.1 Create Line Chart  
- **Category**: Visualization
- **Pre-conditions**: Titanic dataset loaded
- **Post-conditions**: Chart created and rendered
- **Dependencies**: load-titanic-dataset

### 3.1 Apply Data Filters
- **Category**: Data Analysis  
- **Pre-conditions**: Titanic dataset loaded
- **Post-conditions**: Filters applied successfully
- **Dependencies**: load-titanic-dataset

### 4.1 Export Data
- **Category**: Data Export
- **Pre-conditions**: Data loaded
- **Post-conditions**: Export completed, download available
- **Dependencies**: load-titanic-dataset

### 5.1 Keyboard Shortcuts
- **Category**: Accessibility
- **Pre-conditions**: Application accessible
- **Post-conditions**: Keyboard shortcuts functional
- **Dependencies**: None

### 6.1 File Operations
- **Category**: File Management
- **Pre-conditions**: File handling ready
- **Post-conditions**: File operations completed
- **Dependencies**: None

## Usage

### For Developers

1. **Define new scenarios**: Add JSON files to `src/test-scenarios/`
2. **Process scenarios**: Run `npm run test-scenarios`
3. **Test manually**: Open `demo-scenarios.html` in browser
4. **Monitor with Playwright**: Run `npm run test-scenarios:monitor`

### For End Users

1. Open `demo-scenarios.html` in any modern browser
2. Browse available test scenarios
3. Run individual tests or all tests
4. View detailed logs and progress
5. Export results as JSON

## Benefits

### ✅ **Separation of Concerns**
- Test definitions are pure JSON (no code)
- Execution is JavaScript-based (no Playwright complexity)
- Monitoring is Playwright-based (just watching)

### ✅ **Machine-Friendly**
- Clear pre/post conditions
- Structured test steps
- Exportable results
- Consistent pass/fail criteria

### ✅ **Human-Friendly** 
- Visual scenario browser
- Detailed step descriptions
- Progress tracking
- Interactive execution

### ✅ **Maintainable**
- JSON files are version controllable
- Easy to add new scenarios
- Framework handles execution logic
- Clear success/failure reporting

### ✅ **Scalable**
- Ordered execution (1.1, 1.2, 2.1, etc.)
- Dependency management
- Category organization
- Difficulty levels

## File Structure

```
src/test-scenarios/           # JSON scenario definitions
├── 1.1-load-titanic-dataset.json
├── 2.1-create-line-chart.json  
├── 3.1-apply-data-filters.json
├── 4.1-export-data.json
├── 5.1-keyboard-shortcuts.json
└── 6.1-file-operations.json

scripts/
└── process-test-scenarios.js  # Processor script

tests/
└── scenario-monitor.spec.js   # Playwright monitor

demo-scenarios.html           # Generated interactive demo
```

## Commands Summary

```bash
# Process JSON scenarios into interactive HTML
npm run test-scenarios

# Monitor JavaScript execution with Playwright  
npm run test-scenarios:monitor

# Alternative build mode
npm run test-scenarios:build
```

## Future Enhancements

- **Integration with CI/CD**: Automated scenario validation
- **Visual regression testing**: Screenshot comparisons
- **Performance monitoring**: Execution time tracking
- **Custom scenario templates**: Reusable scenario patterns
- **Multi-browser support**: Cross-browser validation

---

**Status**: ✅ Fully implemented and functional  
**Version**: 2.0  
**Last Updated**: August 2025