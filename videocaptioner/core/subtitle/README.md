# Subtitle Rendering Module

Provides two subtitle rendering modes:
- **ASS Styles**: FFmpeg + libass rendering (with CUDA acceleration support)
- **Rounded Backgrounds**: PIL renders modern subtitles with rounded rectangular backgrounds

## Module Structure

```
app/core/subtitle/
├── __init__.py           # Unified public exports
├── ass_renderer.py       # ASS renderer (video composition and previews)
├── ass_utils.py          # ASS parsing and processing with dataclasses
├── rounded_renderer.py   # Rounded-background renderer
├── styles.py             # Style configuration (RoundedBgStyle)
├── font_utils.py         # Font management (bundled/system fonts and LRU caching)
└── text_utils.py         # Text processing (balanced line wrapping)
```

## Quick Start

### 1. ASS Parsing

```python
from app.core.subtitle import parse_ass_info, auto_wrap_ass_file

# Parse an ASS file (returns a type-safe dataclass)
ass_info = parse_ass_info(ass_content)
print(f"Resolution: {ass_info.video_width}x{ass_info.video_height}")
for style in ass_info.styles.values():
    print(f"{style.name}: {style.font_name} {style.font_size}px")

# Smart wrapping based on actual rendered font width
auto_wrap_ass_file("input.ass", video_width=1920)
```

### 2. Rounded-Background Rendering

```python
from app.core.subtitle import render_rounded_video, RoundedBgStyle

style = RoundedBgStyle(
    font_name="Noto Sans SC",
    font_size=52,
    bg_color="#191919C8",      # Translucent dark gray
    text_color="#FFFFFF",
    corner_radius=12,
    letter_spacing=2,          # Letter spacing
)

render_rounded_video(
    video_path="input.mp4",
    asr_data=asr_data,
    output_path="output.mp4",
    style=style,
)
```

### 3. Font and Text Utilities

```python
from app.core.subtitle import get_font, get_ass_to_pil_ratio, wrap_text

# Get a font (prefer bundled fonts, fall back to system fonts)
font = get_font(52, "Noto Sans SC")

# Convert ASS font sizes to PIL font sizes
ratio = get_ass_to_pil_ratio("Noto Sans SC")  # ≈ 1.448
pil_size = int(74 / ratio)  # ASS 74px → PIL 51px

# Balance text wrapping for more even line lengths
lines = wrap_text(text, font, max_width=1216)
```

## Key Features

### Accurate Line Wrapping
- **Actual rendered width**: Use real PIL font rendering instead of estimating character widths
- **Balancing algorithm**: Calculate the minimum number of lines first, then distribute characters evenly to avoid an overly short final line
- **Language-aware wrapping**: Split CJK text by character and English text by word

### Font Management
- **Prefer bundled fonts**: Load fonts from `resource/fonts/` first
- **System font fallback**: Automatically detect system fonts on macOS, Windows, and Linux
- **Cross-platform parsing**: Extract font family names with `fontTools`
- **LRU caching**: Improve performance with the `@lru_cache` decorator

### ASS Font Size Conversion
- **Problem**: ASS uses the Windows line height (usWinAscent + usWinDescent), while PIL uses the em square (unitsPerEm)
- **Solution**: `get_ass_to_pil_ratio()` reads font metrics automatically and calculates the conversion ratio (typically 1.4–1.5)
- **Result**: ASS 74px ≈ PIL 51px (Noto Sans SC), significantly improving wrapping accuracy

## Technical Challenges and Solutions

### 1. Premature Line Wrapping in ASS Text
**Symptom**: Measuring PIL font widths using the ASS font size directly causes lines to wrap too early  
**Cause**: ASS and PIL interpret font size differently (different units)  
**Solution**:
- Read `unitsPerEm` and the Windows line height from the font file
- Calculate the conversion ratio: `ratio = (usWinAscent + usWinDescent) / unitsPerEm`
- Use the converted font size: `pil_size = ass_size / ratio`

### 2. Uneven Subtitle Line Lengths
**Symptom**: Greedy wrapping produces a long first line and a short second line  
**Cause**: Each line is filled with as many characters as possible without considering overall balance  
**Solution**:
- Use a greedy algorithm to calculate the minimum number of lines first
- Calculate the target width: `target = total_width / num_lines`
- Wrap early when the current line reaches 90% of the target width and the next character would exceed 110%
- Balance improves from 50% to 96%

### 3. Type Safety and Code Simplicity
**Problem**: Dictionaries, tuples, and manual caching make the code complex  
**Solution**:
- Use `@dataclass` instead of dictionaries and tuples (`AssInfo`, `AssStyle`)
- Use `@lru_cache` instead of manual cache management
- Use explicit return types (`AssInfo` rather than `tuple[int, Dict[...]]`)

## Notes

1. **Font paths**: Put bundled fonts in `resource/fonts/`; they take priority over system fonts
2. **ASS style wrapping**: Use `\q2` to disable libass automatic wrapping and control all line breaks explicitly
3. **Text width calculation**: The maximum text width defaults to 95% of the video width (`video_width * 0.95`)
4. **Font metric caching**: Results from `get_ass_to_pil_ratio()` are cached to avoid repeated calculation
5. **Rounded-background letter spacing**: Draw character by character when `letter_spacing > 0`; draw the entire string when `= 0` for better performance
