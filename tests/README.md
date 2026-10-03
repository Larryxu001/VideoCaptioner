# Test Suite

Integration tests for the VideoCaptioner translation module.

## 📁 Test Files

```
tests/test_translate/
├── test_google_translator.py   # Google translator (free API)
├── test_bing_translator.py     # Bing translator (free API)
├── test_llm_translator.py      # LLM translator (API key required)
└── test_deeplx_translator.py   # DeepLX translator (optional)
```

## 🚀 Running Tests

### Quick Tests (Free APIs)

```bash
# Google + Bing translators (no configuration required)
uv run pytest tests/test_translate/test_google_translator.py tests/test_translate/test_bing_translator.py -v
```

### Full Tests (API Key Required)

```bash
# 1. Configure environment variables
export OPENAI_BASE_URL=https://api.openai.com/v1
export OPENAI_API_KEY=sk-your-key

# 2. Run all tests
uv run pytest tests/test_translate/ -v
```

### Run Specific Tests

```bash
# Run only the Google translator
uv run pytest tests/test_translate/test_google_translator.py::TestGoogleTranslator::test_translate_simple_text -v

# Skip tests that require an API
uv run pytest tests/test_translate/ -m "not integration" -v
```

## ⚙️ Environment Variables

### Local Development

Create a `.env` file (already covered by .gitignore):

```bash
# LLM translator tests (required)
OPENAI_BASE_URL=https://api.openai.com/v1
OPENAI_API_KEY=sk-your-api-key

# DeepLX translator tests (optional)
DEEPLX_ENDPOINT=https://api.deeplx.org/translate
```

### CI/CD

Configure these in GitHub Actions under **Settings → Secrets**:

- `OPENAI_BASE_URL`
- `OPENAI_API_KEY`
- `DEEPLX_ENDPOINT` (optional)

See [docs/CI_SETUP.md](../docs/CI_SETUP.md)

## 📊 Example Test Results

```
=================== 6 passed, 6 skipped ===================

✅ test_google_translator.py    3 passed
✅ test_bing_translator.py      3 passed
⏭️ test_llm_translator.py       4 skipped (no API key)
⏭️ test_deeplx_translator.py    2 skipped (no endpoint)
```

## 🐛 Troubleshooting

### Tests Are Skipped

**Cause**: Missing environment variables

**Solution**:

```bash
export OPENAI_BASE_URL=...
export OPENAI_API_KEY=...
```

### ImportError

**Cause**: Missing dependencies

**Solution**:

```bash
uv sync --all-extras
```

### Translation Tests Fail

**Cause**: Free APIs may be unstable or rate-limited

**Solution**:

- Google/Bing test failures can occur with these free services
- Wait a few minutes and retry
- Run only the LLM tests for more stable results

## 📝 Adding New Tests

```python
# tests/test_translate/test_my_translator.py
import pytest
from app.core.translate.my_translator import MyTranslator

@pytest.mark.integration
class TestMyTranslator:
    @pytest.fixture
    def translator(self, target_language):
        return MyTranslator(
            thread_num=2,
            batch_num=5,
            target_language=target_language,
            update_callback=None,
        )

    def test_translate(self, translator, sample_asr_data):
        result = translator.translate_subtitle(sample_asr_data)
        assert len(result.segments) == len(sample_asr_data.segments)
        for seg in result.segments:
            assert seg.translated_text  # Ensure a translation is present
```

## 🔗 Related Documentation

- [CI/CD Configuration](../docs/CI_SETUP.md)
- [Testing Guide](../docs/TESTING.md)
