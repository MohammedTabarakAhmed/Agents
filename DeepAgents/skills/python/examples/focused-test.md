# Focused Test Example

This small example demonstrates an explicit contract, dependency-free implementation, temporary test data, and a focused assertion. Production tests should also cover invalid paths, permission failures, encoding errors, and callers that depend on empty-file behavior.

```python
from pathlib import Path


def load_prompt(path: Path) -> str:
    """Load a UTF-8 prompt and remove surrounding whitespace."""
    return path.read_text(encoding="utf-8").strip()


def test_load_prompt_strips_trailing_whitespace(tmp_path: Path) -> None:
    prompt_path = tmp_path / "prompt.txt"
    prompt_path.write_text("hello\n\n", encoding="utf-8")

    assert load_prompt(prompt_path) == "hello"


def test_load_prompt_preserves_internal_whitespace(tmp_path: Path) -> None:
    prompt_path = tmp_path / "prompt.txt"
    prompt_path.write_text(" hello  world ", encoding="utf-8")

    assert load_prompt(prompt_path) == "hello  world"
```

Run the smallest relevant test first: `pytest path/to/test_file.py -q`. Keep live model and cloud calls out of this unit test; inject a fake client for code that performs I/O.
