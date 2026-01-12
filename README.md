# Test Case: Pip Version Critical - DEPENDENCY FAILURE TEST

## Package Manager
Pip

## Python Version Detection
- **Source**: .python-version (PRIORITY #1)
- **Expected Version**: 3.11.0
- **Conflict**: pyproject.toml says >=3.8 (SHOULD BE IGNORED)

## Dependencies (CRITICAL)
- `tomli>=2.0.0` with marker `python_version >= "3.11"`
- `typing-extensions>=4.8.0`

## Test Purpose
**CRITICAL DEPENDENCY TEST WITH ENVIRONMENT MARKERS**

### Why This Test Matters
The `tomli` dependency has an environment marker: `python_version >= "3.11"`

If tool uses **3.11.0** from .python-version:
- ✅ Environment marker evaluates to TRUE
- ✅ tomli is installed
- ✅ All dependencies present

If tool uses **3.8** from pyproject.toml:
- ❌ Environment marker evaluates to FALSE
- ❌ tomli is SKIPPED during installation
- ❌ **Missing dependency** - application will fail at runtime

## Expected Behavior
1. Tool detects .python-version (3.11.0) as highest priority
2. Tool ignores pyproject.toml requires-python (>=3.8)
3. Installs ALL dependencies including tomli
4. Dependency resolution succeeds completely

## Real-World Impact
Environment markers are commonly used for:
- Python version-specific dependencies
- Platform-specific packages
- Optional dependencies based on Python version

**Wrong version detection = silent dependency omission = runtime failures**

This is worse than installation failure - the installation "succeeds" but dependencies are missing!
