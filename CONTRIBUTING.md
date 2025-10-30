# Contributing to NoDetours

Thank you for your interest in contributing to NoDetours! This document provides guidelines and instructions for contributing to the project.

## Code of Conduct

We are committed to providing a welcoming and inclusive environment for all contributors. Please treat all team members and users with respect and professionalism.

---

## Getting Started

### Prerequisites
- Python 3.9+
- Git
- Virtual environment (venv recommended)
- API keys for at least one LLM provider (OpenAI or Anthropic)

### Setup Development Environment

1. **Fork and Clone**
```bash
git clone https://github.com/yourusername/NoDetours.git
cd NoDetours
```

2. **Create Virtual Environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install Dependencies**
```bash
pip install -r requirements.txt
```

4. **Configure Environment**
```bash
cp .env.example .env
# Edit .env with your API keys
```

5. **Run Tests**
```bash
pytest
```

6. **Start Development Server**
```bash
python main.py --config config/config.yaml
# Access at http://localhost:8000
```

---

## Development Workflow

### Creating a Feature Branch

```bash
git checkout -b feature/your-feature-name
# or for bug fixes:
git checkout -b fix/your-bug-description
```

Branch naming conventions:
- `feature/`: New features
- `fix/`: Bug fixes
- `refactor/`: Code refactoring
- `docs/`: Documentation updates
- `test/`: Test additions

### Making Changes

1. **Write Code**
   - Follow PEP 8 style guidelines
   - Add docstrings to all functions and classes
   - Use type hints where applicable
   - Keep functions focused and testable

2. **Write Tests**
   - Add unit tests for new functionality
   - Ensure all tests pass: `pytest`
   - Aim for >80% code coverage for new code

3. **Update Documentation**
   - Update README.md if features change
   - Add docstring examples for complex functions
   - Update TECHNICAL_DECISIONS.md if architecture changes

### Example: Adding a New LLM Provider

1. **Update Configuration Schema**
```yaml
# config/config.yaml
llm_providers:
  new_provider:
    provider: "new_provider_name"
    model: "model_id"
    temperature: 0.7
    max_tokens: 4000
```

2. **Extend LLM Provider Class**
```python
# api/llm_provider.py
class LLMProvider:
    def __init__(self, provider, model, temperature, max_tokens):
        # Add handling for new_provider
        if provider == "new_provider":
            # Initialize new provider API client
            pass
```

3. **Add Tests**
```python
# tests/test_llm_provider.py
def test_new_provider_initialization():
    config = {
        "provider": "new_provider",
        "model": "model_id",
        "temperature": 0.7
    }
    provider = LLMProvider(**config)
    assert provider.provider == "new_provider"
```

4. **Document Changes**
- Update README.md with new provider info
- Add provider comparison in BENCHMARKS.md
- Document integration in TECHNICAL_DECISIONS.md

---

## Code Style & Standards

### Python Style Guide
- Follow [PEP 8](https://www.python.org/dev/peps/pep-0008/)
- Use meaningful variable names
- Maximum line length: 100 characters
- Use 4 spaces for indentation

### Docstring Format
```python
def extract_features(user_input: str) -> Dict[str, Any]:
    """
    Extract travel preferences from user input.

    Analyzes natural language input to identify key travel parameters
    like destination, duration, budget level, and activity preferences.

    Args:
        user_input (str): User's travel plan description in natural language.
            Example: "Help me plan a 5-day trip to Japan focusing on anime sites"

    Returns:
        Dict[str, Any]: Extracted features with keys:
            - destination (str): Primary travel destination
            - duration (int): Trip length in days
            - budget_level (str): 'budget', 'moderate', or 'luxury'
            - interests (List[str]): Activity interests
            - preferences (Dict): Additional preferences

    Raises:
        ValueError: If input is empty or too short to extract features

    Example:
        >>> features = extract_features("7 days in Paris, museums and food")
        >>> features['destination']
        'Paris'
        >>> features['duration']
        7
    """
```

### Type Hints
```python
from typing import Dict, List, Optional, Any
from pydantic import BaseModel

class TravelPreferences(BaseModel):
    destination: str
    duration: int
    budget_level: str = "moderate"
    interests: List[str] = []
    flexibility: Optional[str] = None

def process_preferences(prefs: TravelPreferences) -> Dict[str, Any]:
    """Process travel preferences into structured data."""
    pass
```

---

## Testing

### Running Tests
```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=app --cov=api tests/

# Run specific test file
pytest tests/test_features.py

# Run specific test
pytest tests/test_features.py::test_extract_features

# Run with verbose output
pytest -v
```

### Writing Tests

```python
# tests/test_features.py
import pytest
from app.modules.search_query_extractor import extract_features

class TestFeatureExtraction:
    """Test suite for feature extraction module."""

    def test_extract_destination(self):
        """Test destination extraction from user input."""
        user_input = "I want to visit Paris"
        features = extract_features(user_input)
        assert features['destination'] == "Paris"

    def test_extract_duration(self):
        """Test duration extraction."""
        user_input = "5-day trip to Tokyo"
        features = extract_features(user_input)
        assert features['duration'] == 5

    def test_invalid_input(self):
        """Test handling of invalid input."""
        with pytest.raises(ValueError):
            extract_features("")

    @pytest.mark.parametrize("input_text,expected_budget", [
        ("Luxury trip to Maldives", "luxury"),
        ("Budget backpacking in SE Asia", "budget"),
        ("Comfortable trip to Rome", "moderate"),
    ])
    def test_budget_level(self, input_text, expected_budget):
        """Test budget level extraction for various inputs."""
        features = extract_features(input_text)
        assert features['budget_level'] == expected_budget
```

### Test Coverage Goals
- New code: >80% coverage
- Existing code: Maintain current coverage
- Critical paths: 100% coverage
- Integration tests: One per API endpoint

---

## Commit Message Guidelines

Follow conventional commit format:

```
type(scope): subject

body

footer
```

### Types
- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation changes
- **style**: Code style changes (no functional changes)
- **refactor**: Code refactoring without feature changes
- **test**: Adding or updating tests
- **chore**: Build process, dependencies, etc.

### Examples

```bash
# Good commit message
git commit -m "feat(llm): add support for Claude 3.5 Sonnet

- Implement new LLMProvider configuration for Claude 3.5 Sonnet
- Add cost optimization comparison to BENCHMARKS.md
- Update provider selection guidelines in documentation

Closes #42"

# Another example
git commit -m "fix(context-collector): handle weather API timeout gracefully

- Add 3-second timeout with retry logic
- Log failures for monitoring
- Test with mocked API failures

Related to #38"
```

---

## Pull Request Process

### Before Creating PR
1. Ensure all tests pass: `pytest`
2. Check code coverage doesn't decrease
3. Run type checking: `mypy app/ api/` (if configured)
4. Update documentation as needed
5. Rebase on latest main branch

### Creating PR

1. **Push to your fork**
```bash
git push origin feature/your-feature-name
```

2. **Open Pull Request**
   - Clear title following commit message format
   - Detailed description of changes
   - Link related issues
   - Include before/after if UI changes

3. **PR Template**
```markdown
## Description
Brief description of changes.

## Type of Change
- [ ] New feature
- [ ] Bug fix
- [ ] Breaking change
- [ ] Documentation update

## Changes
- Change 1
- Change 2

## Testing
How to test these changes:
1. Step 1
2. Step 2

## Screenshots (if applicable)
[Add screenshots for UI changes]

## Checklist
- [ ] Tests pass locally
- [ ] Code follows style guidelines
- [ ] Documentation updated
- [ ] No new warnings generated
- [ ] Changes don't break existing tests

## Related Issues
Closes #123
```

### Review Process
1. At least one review required
2. All conversations must be resolved
3. All checks must pass
4. Squash and merge to main

---

## Architecture & Design Principles

### When Making Architectural Changes

1. **Discuss First**
   - Open an issue describing the proposal
   - Include rationale and trade-offs
   - Wait for feedback before implementing

2. **Document Decision**
   - Update TECHNICAL_DECISIONS.md
   - Explain why and alternatives considered
   - Document trade-offs accepted

3. **Update Architecture Diagram**
   - Modify visual representation in README
   - Ensure documentation reflects changes

4. **Comprehensive Tests**
   - Tests for new architectural pattern
   - Integration tests across boundaries
   - Performance tests if applicable

### Key Principles
- **Modularity**: Keep concerns separated
- **Testability**: Design for unit testing
- **Configuration**: Make behavioral decisions configurable
- **Graceful Degradation**: Don't fail catastrophically
- **Observability**: Log important events
- **Documentation**: Every decision should be documented

---

## Adding New Features

### Step-by-Step Process

1. **Open an Issue**
```markdown
## Feature: [Feature Name]

**Description**: What problem does this solve?

**Use Cases**: How would users benefit?

**Implementation**: Any initial thoughts on approach?
```

2. **Get Feedback**
   - Discuss with maintainers
   - Refine requirements
   - Align on approach

3. **Create Detailed Plan**
   - Module structure
   - New configuration needed
   - Test plan
   - Documentation requirements

4. **Implement Incrementally**
   - Small, reviewable commits
   - Each commit should be functional
   - Tests included with each commit

5. **Create Pull Request**
   - Link to original issue
   - Explain implementation choices
   - Include examples

---

## Documentation

### What Needs Documentation

1. **README.md**: User-facing documentation
2. **TECHNICAL_DECISIONS.md**: Why decisions were made
3. **Code Comments**: Complex logic
4. **Docstrings**: Every function and class
5. **Examples**: Usage patterns and integration examples

### Documentation Standards

- **Clear Language**: Write for your future self 6 months from now
- **Examples**: Include code examples for features
- **Caveats**: Document known limitations and workarounds
- **Links**: Cross-reference related documentation

---

## Performance & Optimization

### When Optimizing Code

1. **Measure First**
   - Profile the code
   - Identify actual bottlenecks
   - Document baseline metrics

2. **Make Changes**
   - Small, focused changes
   - Verify improvement with metrics
   - Include benchmarks in PR

3. **Document Impact**
   - Update BENCHMARKS.md
   - Show before/after metrics
   - Explain optimization technique

### Performance Guidelines
- Target: <10 second total response time
- Acceptable: <5 second p95 latency
- Parallel I/O operations strongly preferred
- Cache when beneficial (document in BENCHMARKS.md)

---

## Reporting Issues

### Security Issues
**Do not open public issues for security vulnerabilities**

Email security details privately to: [maintainer-email]

Include:
- Description of vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if available)

### Bug Reports

```markdown
## Description
Brief description of the bug.

## Reproduction Steps
1. Step 1
2. Step 2
3. ...

## Expected Behavior
What should happen?

## Actual Behavior
What actually happens?

## Environment
- Python version: 3.9+
- OS: macOS / Linux / Windows
- LLM Provider: GPT-4 / Claude / etc.

## Logs
[Add relevant error messages]

## Screenshots
[Add if applicable]
```

### Feature Requests

```markdown
## Description
What would you like to add?

## Use Case
Why do you need this feature?

## Proposed Solution
How should this work?

## Alternatives
Any other approaches?

## Additional Context
Links, screenshots, examples?
```

---

## Development Tips

### Useful Commands

```bash
# Run application
python main.py --config config/config.yaml

# Run CLI mode
python main.py --cli

# Run evaluation pipeline
python run_evaluation.py --config config/eval_config.yaml

# Format code with black (if installed)
black app/ api/

# Type check with mypy
mypy app/ api/ --ignore-missing-imports

# Lint with flake8
flake8 app/ api/ --max-line-length=100

# Generate coverage report
pytest --cov=app --cov=api --cov-report=html
```

### Debugging Tips

```python
# Add logging for debugging
import logging
logger = logging.getLogger(__name__)

logger.debug(f"Feature extracted: {features}")
logger.info("Starting context collection")
logger.warning("API timeout, using fallback")
logger.error(f"Failed to parse response: {error}")

# Use pdb for interactive debugging
import pdb; pdb.set_trace()

# Or use Python debugger in modern Python
breakpoint()
```

### Common Development Tasks

**Adding a new API integration**:
1. Create wrapper in `api/[service].py`
2. Add configuration to `config/config.yaml`
3. Update `context_collector.py` to use new service
4. Add tests in `tests/test_api_[service].py`
5. Document in README and TECHNICAL_DECISIONS.md

**Modifying evaluation metrics**:
1. Update `config/eval_config.yaml`
2. Modify evaluation prompt in `evaluator.py`
3. Re-run evaluation pipeline: `python run_evaluation.py`
4. Update BENCHMARKS.md with new results
5. Document rationale in PR

**Optimizing response time**:
1. Profile current performance: `python -m cProfile main.py`
2. Identify bottleneck
3. Implement optimization
4. Measure improvement
5. Update BENCHMARKS.md with new metrics
6. Document optimization in TECHNICAL_DECISIONS.md

---

## Review Checklist for Maintainers

When reviewing PRs, check:

- [ ] Code follows style guidelines
- [ ] All tests pass and coverage maintained
- [ ] Documentation updated appropriately
- [ ] Commit messages follow convention
- [ ] Architecture decisions align with principles
- [ ] Performance acceptable (no unexpected degradation)
- [ ] No hardcoded values (use configuration)
- [ ] Error handling appropriate
- [ ] Logging sufficient for debugging
- [ ] Changes don't introduce security issues

---

## Release Process

### Version Numbering
Follow [Semantic Versioning](https://semver.org/):
- MAJOR: Breaking changes
- MINOR: New features (backward compatible)
- PATCH: Bug fixes

### Release Checklist
1. Update version in `config/config.yaml`
2. Update CHANGELOG.md
3. Create git tag: `git tag v1.0.0`
4. Push tag: `git push origin v1.0.0`
5. Update release notes on GitHub

---

## Getting Help

### Questions?
- Open an issue for questions
- Check existing issues for similar questions
- Ask in discussions tab

### Resources
- [API Documentation](README.md#api-documentation)
- [Technical Decisions](TECHNICAL_DECISIONS.md)
- [Benchmarks & Performance](BENCHMARKS.md)
- [Configuration Guide](config/config.yaml)

---

## Recognition

Contributors are recognized in:
- GitHub: Automatically in contributor list
- CHANGELOG.md: Mentioned in releases
- README.md: Listed in acknowledgments (for significant contributions)

Thank you for contributing to NoDetours! 🚀
