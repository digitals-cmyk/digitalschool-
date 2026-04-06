```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

# ADD THIS ENV BLOCK TO FIX THE WARNING
env:
  FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '22' # Your project's Node version
          
      - name: Install dependencies
        run: npm ci
        
      - name: Build
        run: npm run build

      - name: Upload Artifact
        uses: actions/upload-artifact@v4
        with:
          name: build-artifacts
          path: dist/
```

### Option 2: Apply to a Specific Job

If you only want to apply this to a specific job, you can place the `env` block inside the job definition:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    # ADD THIS ENV BLOCK TO THE JOB
    env:
      FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true
    steps:
      - uses: actions/checkout@v4
      # ... rest of your steps
```

## Long-Term Fix

Eventually, GitHub will release `actions/checkout@v5` and `actions/upload-artifact@v5` (or update the `v4` tags) to natively use Node.js 24. 

Once those are available, you can update your `uses:` statements and remove the `FORCE_JAVASCRIPT_ACTIONS_TO_NODE24` environment variable:

```yaml
    steps:
      - name: Checkout repository
        uses: actions/checkout@v5 # Update to v5 when available
```

## Temporary Opt-Out (Not Recommended)

If opting into Node 24 breaks your workflow for some reason, you can temporarily force the runner to keep using the insecure Node version until September 16th, 2026:

```yaml
env:
  ACTIONS_ALLOW_USE_UNSECURE_NODE_VERSION: true
```
