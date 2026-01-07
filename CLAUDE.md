# RenderCV Project Guide

This document provides comprehensive patterns and guidelines for working with RenderCV in this CV repository.

## Overview

RenderCV is a CV/resume generator that uses YAML input files to generate professional CVs in multiple formats (PDF, HTML, Markdown, PNG). It uses Typst for PDF rendering and supports multiple themes and locales.

**Project Structure:**
- `languages/` - Contains YAML CV files, one per language (e.g., `en.yaml`, `pt.yaml`)
- File naming convention: `{language_code}.yaml` (e.g., `en.yaml` for English, `pt.yaml` for Portuguese)
- Build outputs are saved as `{language_code}.pdf` and `{language_code}.html`

---

## YAML Input Structure

A RenderCV YAML file has 4 main top-level fields:

```yaml
cv:          # Personal information and CV content
design:      # Visual styling and theme
locale:      # Language and date formatting
settings:    # Output paths and generation options
```

---

## 1. CV Field

The `cv` field contains all personal information and CV content.

### Header Information (Personal Details)

All header fields are optional:

```yaml
cv:
  name: Your Full Name
  headline: Professional Title or Tagline
  location: City, Country
  email: your.email@example.com
  phone: +1-555-555-5555
  website: https://yourwebsite.com
  photo: path/to/photo.jpg

  social_networks:
    - network: LinkedIn      # See supported networks below
      username: your-username
    - network: GitHub
      username: your-username

  custom_connections:
    - placeholder: Custom Label
      url: https://example.com
      fontawesome_icon: fa-icon-name
```

**Supported Social Networks:**
- LinkedIn, GitHub, GitLab, ORCID, ResearchGate, Google Scholar
- StackOverflow, Leetcode
- Instagram, YouTube, Telegram, WhatsApp
- X (Twitter), Bluesky, Mastodon
- IMDB

**Note:** Multiple values supported for `email`, `phone`, and `website` (use list format).

### Sections Structure

Sections are defined as a dictionary where keys are section titles (displayed as headings) and values are lists of entries:

```yaml
cv:
  sections:
    Experience:           # Section title (customizable)
      - company: ...      # Entry 1
      - company: ...      # Entry 2

    Education:
      - institution: ...

    Skills:
      - label: ...
```

**Important Rules:**
- Each section must contain only ONE entry type
- Section titles are customizable (use any name you want)
- Entries are displayed in the order they appear

---

## 2. Entry Types

RenderCV provides 9 entry types. Each has required and optional fields.

### ExperienceEntry
For work experience, internships, volunteer work.

**Required:** `company`, `position`
**Optional:** `date`, `start_date`, `end_date`, `location`, `summary`, `highlights`

```yaml
sections:
  experience:
    - company: Hospital Israelita Albert Einstein
      position: Senior Frontend Developer
      location: São Paulo, Brazil
      start_date: 2021-08
      end_date: present
      highlights:
        - Implemented a dynamic medical forms framework
        - Standardized and modernized MFE architecture
        - Integrated medical record services
```

### EducationEntry
For academic qualifications.

**Required:** `institution`, `area`
**Optional:** `degree`, `date`, `start_date`, `end_date`, `location`, `summary`, `highlights`

```yaml
sections:
  education:
    - institution: FIAP
      area: Information Systems
      degree: Bachelor's Degree
      start_date: 2020
      end_date: 2025
      location: São Paulo, Brazil
```

### NormalEntry
Generic entry for projects, certifications, awards.

**Required:** `name`
**Optional:** `date`, `start_date`, `end_date`, `location`, `summary`, `highlights`

```yaml
sections:
  projects:
    - name: Open Source Project
      date: 2023
      summary: Description of the project
      highlights:
        - Key achievement 1
        - Key achievement 2
```

### PublicationEntry
For research papers, articles, publications.

**Required:** `title`, `authors`
**Optional:** `doi`, `url`, `journal`, `date`

```yaml
sections:
  publications:
    - title: Paper Title
      authors: First Author, Second Author, Third Author
      doi: 10.1234/example
      journal: Journal Name
      date: 2023-05
```

### OneLineEntry
Single-line key-value pair (commonly used for skills).

**Required:** `label`, `details`

```yaml
sections:
  skills:
    - label: Core Competencies
      details: JavaScript, React.js, Software Development
    - label: Frameworks & Libraries
      details: React, Next.js, Remix, Astro, Angular, Node.js
    - label: Architecture
      details: Micro Frontends (MFE)
```

### BulletEntry
Simple bullet point.

**Required:** `bullet`

```yaml
sections:
  achievements:
    - bullet: Won first place in hackathon
    - bullet: Published article in technical magazine
```

### TextEntry
Plain text paragraph without structure.

```yaml
sections:
  summary:
    - Senior front-end developer with solid experience in scalable web applications.
```

### NumberedEntry & ReversedNumberedEntry
For numbered lists.

**Required:** `number` or `reversed_number`

```yaml
sections:
  certifications:
    - number: AWS Certified Solutions Architect
    - number: Google Cloud Professional
```

---

## 3. Date Formatting

Dates can be specified in multiple formats:

```yaml
# Full date
date: 2023-05-15

# Year-month (most common)
start_date: 2021-08
end_date: 2023-12

# Year only
date: 2023

# Present/ongoing (uses locale setting)
end_date: present
```

**Date Display:**
- RenderCV automatically formats dates based on locale settings
- Calculates duration between `start_date` and `end_date`
- Displays "present" or locale-specific equivalent for ongoing positions

---

## 4. Text Formatting

RenderCV supports Markdown and Typst formatting in text fields.

### Markdown Support

```yaml
highlights:
  - "**Bold text** for emphasis"
  - "*Italic text* for nuance"
  - "[Link text](https://example.com) for URLs"
  - "`code` for inline code or technical terms"
```

### Typst Support

```yaml
summary: "Mathematical expression: $$f(x) = x^2$$"
highlights:
  - "#emph[Emphasized text] using Typst"
```

---

## 5. Design Field

The `design` field controls visual styling and theming.

### Built-in Themes

RenderCV provides 5 built-in themes:

- `classic` (default) - Traditional professional CV
- `engineeringclassic` - Engineering-focused classic style
- `engineeringresumes` - Modern engineering resume
- `moderncv` - Contemporary CV style
- `sb2nov` - Popular GitHub resume template style

```yaml
design:
  theme: classic
```

**Note:** All themes are structurally identical but have different default styling values.

### Color Customization

You can override any color in the theme:

```yaml
design:
  theme: classic
  colors:
    name: rgb(0, 79, 144)              # Name color
    connections: rgb(0, 79, 144)        # Contact links color
    section_titles: rgb(0, 79, 144)     # Section heading color
    links: rgb(0, 79, 144)              # Hyperlink color
    body: black                         # Body text color
    headline: black                     # Headline/tagline color
    footer: black                       # Footer text color
    top_note: black                     # Top note color
```

**Color Formats:**
- `rgb(r, g, b)` - RGB values (0-255)
- Named colors: `black`, `white`, `red`, `blue`, etc.
- Hex colors: `#004F90`

### Page Settings

```yaml
design:
  page:
    size: a4                    # a4, a5, us-letter, us-executive
    top: 2 cm
    bottom: 2 cm
    left: 2 cm
    right: 2 cm
    show_footer: true
    show_top_note: false
```

### Typography

```yaml
design:
  text:
    # Line spacing
    line_spacing: 0.6 em        # Space between lines

    # Alignment
    alignment: justified-with-no-hyphenation
    # Options: left, justified, justified-with-no-hyphenation

    # Font family (30+ presets available)
    font_family: Source Sans 3
    # Popular: Roboto, Source Sans 3, Helvetica, Arial, Times New Roman

    # Font sizes
    font_size: 10 pt            # Body text
    name_font_size: 30 pt
    headline_font_size: 14 pt
    connections_font_size: 10 pt
    section_title_font_size: 14 pt

    # Styling toggles
    use_small_caps_for_section_titles: true
    bold_name: true
```

### Header Configuration

```yaml
design:
  header:
    alignment: center           # left, center, right

    # Photo settings
    photo_width: 3.5 cm
    photo_position: left        # left or right
    photo_spacing: 0.5 cm

    # Spacing
    space_below_name: 0.2 cm
    space_below_headline: 0.2 cm
    space_below_connections: 0.5 cm
```

### Section & Entry Styling

```yaml
design:
  section_titles:
    # Style options:
    # - with_partial_line (default)
    # - with_full_line
    # - without_line
    # - moderncv
    style: with_partial_line

    space_above: 0.4 cm
    space_below: 0.2 cm

  entries:
    # Spacing between entries
    space_between_entries: 0.5 cm
    space_between_text_entries: 0.2 cm

    # Bullet style
    bullet_style: ●
    # Options: ●, •, ◦, -, ◆, ★, ■, —, ○

    # Page breaks
    allow_page_breaks_in_entries: true
```

### Advanced Customization

You can create custom entry templates using placeholders:

```yaml
design:
  entry_types:
    ExperienceEntry:
      template: |
        **POSITION** at **COMPANY** (LOCATION)
        DATE
        HIGHLIGHTS
```

**Available Placeholders:** NAME, COMPANY, POSITION, DATE, LOCATION, SUMMARY, HIGHLIGHTS, etc.

---

## 6. Locale Field

The `locale` field enables multi-language CV creation.

### Built-in Languages

RenderCV supports 12 languages:

- `english` (default)
- `danish`, `dutch`, `french`, `german`
- `hindi`, `indonesian`, `italian`
- `japanese`, `korean`, `mandarin_chinese`
- `portuguese`, `russian`, `spanish`, `turkish`

```yaml
locale:
  language: portuguese
```

### Customizable Locale Fields

Override specific fields while using a built-in language:

```yaml
locale:
  language: portuguese
  present: presente           # Override "present" term
  month_abbreviations:        # Custom month abbreviations
    - jan
    - fev
    - mar
    - abr
    - maio
    - jun
    - jul
    - ago
    - set
    - out
    - nov
    - dez
```

### Complete Custom Locale

For full customization, specify all fields:

```yaml
locale:
  last_updated: "Last updated on"
  present: "present"
  month: "month"
  months: "months"
  year: "year"
  years: "years"
  month_abbreviations:
    - Jan
    - Feb
    - Mar
    - Apr
    - May
    - Jun
    - Jul
    - Aug
    - Sep
    - Oct
    - Nov
    - Dec
  month_names:
    - January
    - February
    - March
    - April
    - May
    - June
    - July
    - August
    - September
    - October
    - November
    - December
```

---

## 7. Settings Field

The `settings` field controls RenderCV's behavior and output generation.

### Output Paths

Customize where files are generated:

```yaml
settings:
  typst_path: output/NAME_IN_KEBAB_CASE.typ
  pdf_path: output/NAME_IN_KEBAB_CASE.pdf
  markdown_path: output/NAME_IN_KEBAB_CASE.md
  html_path: output/NAME_IN_KEBAB_CASE.html
  png_path: output/NAME_IN_KEBAB_CASE.png
```

**Available Placeholders:**
- `NAME` - Full name as-is
- `NAME_IN_SNAKE_CASE` - john_doe
- `NAME_IN_KEBAB_CASE` - john-doe
- `NAME_IN_UPPERCASE` - JOHN DOE
- `MONTH_NAME` - January
- `YEAR` - 2024
- `YEAR_IN_TWO_DIGITS` - 24

### External Configuration Files

Load design, locale, or settings from separate files:

```yaml
settings:
  design: path/to/design.yaml
  locale: path/to/locale.yaml
```

### Generation Toggles

Control which file formats are generated:

```yaml
settings:
  dont_generate_markdown: false
  dont_generate_html: false
  dont_generate_typst: false
  dont_generate_pdf: false
  dont_generate_png: true        # PNG generation disabled
```

### Text Formatting

Automatically bold specific keywords throughout the CV:

```yaml
settings:
  bold_keywords:
    - React
    - TypeScript
    - Python
    - AWS
```

### Date Settings

Set reference date for calculations:

```yaml
settings:
  current_date: 2024-01-15
```

This affects:
- File naming when using date placeholders
- "Last updated" text generation
- Duration calculations for ongoing positions

---

## 8. CLI Commands

### Generate Sample CV

```bash
# Create new CV with default theme
rendercv new "John Doe"

# Create with specific theme
rendercv new "John Doe" --theme moderncv

# Create with specific locale
rendercv new "John Doe" --locale portuguese

# Generate with custom templates (advanced)
rendercv new "John Doe" --create-typst-templates
```

### Render CV

```bash
# Basic rendering (generates all formats)
rendercv render cv.yaml

# Watch mode (auto-rebuild on changes)
rendercv render cv.yaml --watch

# Quiet mode (suppress messages)
rendercv render cv.yaml --quiet

# Custom output paths
rendercv render cv.yaml --pdf-path output/cv.pdf

# Skip specific formats
rendercv render cv.yaml --dont-generate-png --dont-generate-markdown

# External configuration files
rendercv render cv.yaml --design design.yaml --locale-catalog locale.yaml

# Override YAML values via CLI (dot notation)
rendercv render cv.yaml --cv.phone "+1-555-555-5555"
rendercv render cv.yaml --cv.sections.education.0.institution "MIT"
rendercv render cv.yaml --design.theme "moderncv"
```

### Create Custom Theme

```bash
# Generate custom theme template files
rendercv create-theme "mytheme"
```

### Version & Help

```bash
# Check version
rendercv --version

# Show help
rendercv --help
```

---

## 9. Project-Specific Patterns

### Multi-Language CV Structure

This project uses a multi-language approach:

1. **File Naming:** `languages/{language_code}.yaml`
   - Example: `languages/en.yaml`, `languages/pt.yaml`

2. **Language Code Mapping:** Build process creates `lang_mapping.txt`
   - Format: `LANG_CODE:ORIGINAL_PDF_NAME`
   - Example: `en:Guilherme_Nimer_Gojlevicius_CV.pdf`

3. **Output Naming:** Files are saved as `{language_code}.pdf`
   - Example: `en.pdf`, `pt.pdf`, `en.html`, `pt.html`

### Common Pattern: Same Person, Multiple Languages

When creating CVs in multiple languages for the same person:

```yaml
# languages/en.yaml
cv:
  name: Guilherme Nimer Gojlevicius
  location: São Paulo, Brazil
  headline: Senior Frontend Developer
  summary: Senior front-end developer with...

locale:
  language: english

# languages/pt.yaml
cv:
  name: Guilherme Nimer Gojlevicius
  location: São Paulo, Brasil
  headline: Desenvolvedor Frontend Sênior
  summary: Desenvolvedor front-end sênior com...

locale:
  language: portuguese
  present: presente
  month_abbreviations: [jan, fev, mar, ...]
```

**Key Points:**
- Keep structure identical across languages
- Translate all text content (summary, highlights, etc.)
- Use appropriate locale settings
- Maintain same design/colors for brand consistency

### Workflow Integration

The GitHub Actions workflow:

1. **Discovers** all YAML files in `languages/` directory
2. **Builds** each CV using `rendercv render {language}.yaml`
3. **Renames** outputs to `{language_code}.pdf` and `{language_code}.html`
4. **Creates** `lang_mapping.txt` for tracking
5. **Deploys** to GitHub Pages with clean URLs

---

## 10. Best Practices

### Content Organization

1. **Order entries reverse-chronologically** (newest first)
2. **Use highlights** for key achievements, not job duties
3. **Be specific** with metrics and results when possible
4. **Keep consistent** verb tenses (past for completed, present for ongoing)

### Styling Guidelines

1. **Choose ONE theme** and customize colors if needed
2. **Keep design consistent** across language versions
3. **Use brand colors** for name, connections, and section titles
4. **Test readability** with different font sizes

### Multi-Language Tips

1. **Maintain parallel structure** across language versions
2. **Translate idioms** appropriately, not literally
3. **Adjust date formats** per locale (MM/DD/YYYY vs DD/MM/YYYY)
4. **Use native terminology** for job titles and qualifications

### Performance

1. **Disable unused formats** with `dont_generate_*` settings
2. **Use watch mode** during development for instant feedback
3. **Keep images optimized** if using photo field

### Version Control

1. **Commit YAML files** to repository
2. **Don't commit** generated outputs (PDF, HTML, etc.)
3. **Use `.gitignore`** for `rendercv_output/` directory
4. **Track design separately** if shared across multiple CVs

---

## 11. Common Issues & Solutions

### Issue: PDF not generating
**Solution:** Ensure Typst is installed and accessible. Check `rendercv --version`.

### Issue: Custom fonts not working
**Solution:** Use fonts from the 30+ presets, or install fonts system-wide.

### Issue: Date calculations wrong
**Solution:** Set `current_date` in settings field explicitly.

### Issue: Entries not displaying
**Solution:** Check that all entries in a section use the same entry type.

### Issue: Markdown formatting not rendering
**Solution:** Use proper syntax and ensure no conflicting Typst commands.

### Issue: Colors not applying
**Solution:** Use `rgb(r, g, b)` format or valid color names. Check theme conflicts.

---

## 12. Resources

- **Official Documentation:** https://docs.rendercv.com/
- **GitHub Repository:** https://github.com/sinaatalay/rendercv
- **Issue Tracker:** https://github.com/sinaatalay/rendercv/issues

---

## 13. Quick Reference

### Minimal CV Example

```yaml
cv:
  name: Your Name
  email: your.email@example.com
  sections:
    Experience:
      - company: Company Name
        position: Job Title
        start_date: 2020-01
        end_date: present
        highlights:
          - Achievement 1
          - Achievement 2
```

### Multi-Language CV Template

```yaml
cv:
  name: Your Name
  location: City, Country
  email: email@example.com
  social_networks:
    - network: LinkedIn
      username: your-username
  headline: Professional Title
  summary: Professional summary...

  sections:
    experience:
      - company: Company
        position: Position
        start_date: YYYY-MM
        end_date: present
        highlights:
          - Key achievement

    education:
      - institution: University
        area: Field of Study
        degree: Degree Type
        start_date: YYYY
        end_date: YYYY

    skills:
      - label: Category
        details: Skill1, Skill2, Skill3

design:
  theme: classic
  colors:
    name: rgb(0, 79, 144)
    connections: rgb(0, 79, 144)
    section_titles: rgb(0, 79, 144)
    links: rgb(0, 79, 144)

locale:
  language: english

settings:
  dont_generate_png: true
```
