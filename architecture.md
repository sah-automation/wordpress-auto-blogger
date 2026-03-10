# Project Architecture: WordPress Auto-Blogging Workflow

This project implements an advanced, AI-driven automated blogging system using **n8n**. It automates the entire content lifecycle—from research and planning to writing, SEO optimization, and publishing to WordPress.

## System Overview

The system is designed as a multi-stage pipeline where different specialized AI models handle specific tasks to ensure high-quality, SEO-optimized content.

### Core Components
- **Orchestration**: [n8n](https://n8n.io/)
- **Input Source**: Google Sheets (managed via the `Post Input` node)
- **AI Models & Roles**:
    - **DeepSeek R1**: Preliminary and Detailed Content Planning.
    - **Perplexity AI**: Structured, real-time web research and citation gathering.
    - **Claude 3.5 Sonnet**: Long-form blog post writing.
    - **Google Gemini 2.5 Flash**: HTML formatting, slug generation, title extraction, and meta-description generation.
- **Image Sourcing**: SerpAPI (Google Images search).
- **Destination**: WordPress (REST API).

## Workflow Stages

1.  **Trigger & Input**: The workflow starts with a manual trigger and fetches the next "incomplete" post from a Google Sheet.
2.  **Preliminary Planning**: DeepSeek R1 generates a content angle, target audience refinement, and a rough structure.
3.  **Research**: Perplexity performs live research on the topic, analyzing competitors and gathering data points/citations.
4.  **Detailed Planning**: DeepSeek R1 combines the preliminary plan and research into a comprehensive article outline.
5.  **Content Writing**: Claude 3.5 Sonnet writes the full article based on the detailed outline, maintaining a professional B2B tone.
6.  **Formatting**: Gemini converts the Markdown content into styled HTML, applying custom CSS for consistent branding (colors, fonts, borders).
7.  **SEO Optimization**: Gemini generates an SEO-friendly slug, title, and meta description based on the final content and target keywords.
8.  **Media Integration**: The system searches for a relevant cover image using SerpAPI.
9.  **Publishing**: A draft post is created in WordPress containing the styled HTML content, cover image, and SEO metadata.

## Data Model (Input CSV)

The system expects the following input fields (as seen in `data/wordpress-auto-blog-sample-data.csv`):

| Field | Description |
| :--- | :--- |
| **Post Type** | Pillar or Cluster post. |
| **Pillar** | The main topic pillar. |
| **Cluster** | The sub-topic cluster (if applicable). |
| **Intent** | Search intent (Informational, Buying, Comparison, etc.). |
| **Primary Keyword** | Main SEO keyword to target. |
| **Brand Focus** | Specific brands to highlight in the content. |
| **Secondary Keywords**| Supporting keywords for SEO. |
| **Target Audience** | Roles and business types. |
| **Pain Points** | Motivations and challenges of the audience. |
| **Word Count** | Desired length of the article. |

## Directory Structure

```text
.
├── data/           # Sample input data and templates
├── screenshot/     # Visual representation of the n8n workflow
├── workflow/       # The .json export of the n8n workflow
└── architecture.md # This documentation
```

## Setup Requirements

- **n8n Instance**: Self-hosted or Cloud.
- **API Keys**:
    - Google Cloud (Gemini & Sheets)
    - OpenRouter (DeepSeek & Claude)
    - Perplexity
    - SerpAPI
    - WordPress (Application Password)
