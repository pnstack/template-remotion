# GitHub Actions - Video Generation Guide

This repository includes automated workflows for generating videos using GitHub Actions. You can render videos directly from GitHub without any local setup.

## Available Workflows

### 1. Generate Video (Simple) 🎬

**File**: `.github/workflows/generate-video.yml`

This is the easiest way to generate videos. Simply select a pre-configured composition and render it.

#### How to Use

1. Navigate to your repository on GitHub
2. Click on the **Actions** tab
3. Select **Generate Video** from the workflow list
4. Click **Run workflow** button (top right)
5. Fill in the inputs:
   - **Composition ID**: Select from dropdown (required)
   - **Output filename**: Name your video file (optional, default: `video.mp4`)
   - **Video quality**: CRF value 0-51 (optional, default: `18`)
6. Click **Run workflow** to start
7. Wait for completion (typically 1-3 minutes)
8. Download your video from the workflow artifacts

#### Available Compositions

**News Stories:**
- `NewsStory` - Original news story template
- `BreakingNews` - Breaking news format
- `TechNews` - Technology news
- `SportsNews` - Sports news
- `MultiSegmentTest` - Multi-segment story
- `MultiStory` - Multiple stories with transitions
- `QuickNews` - Quick news compilation

**Quote Videos:**
- `MotivationalQuote` - Steve Jobs quote
- `WisdomQuote` - Robert Frost quote
- `SuccessQuote` - Winston Churchill quote
- `InspirationQuote` - Theodore Roosevelt quote
- `HappinessQuote` - Dalai Lama quote
- `SimpleQuote` - Oscar Wilde quote
- `QuoteWithAudio` - Quote with background music

#### Quality Settings

The CRF (Constant Rate Factor) parameter controls video quality:
- **0** - Lossless (very large file)
- **18** - Visually lossless (default, recommended)
- **23** - High quality
- **28** - Medium quality (smaller file)
- **51** - Worst quality (smallest file)

### 2. Generate Custom Video (Advanced) 🎨

**File**: `.github/workflows/generate-custom-video.yml`

For advanced users who want to customize video content with their own data.

#### How to Use

1. Navigate to your repository on GitHub
2. Click on the **Actions** tab
3. Select **Generate Custom Video** from the workflow list
4. Click **Run workflow** button
5. Fill in the inputs:
   - **Composition type**: `news` or `quote`
   - **Custom props JSON**: Your custom data (see examples below)
   - **Output filename**: Name your video file
   - **Duration frames**: Video length (300 frames = 10 seconds at 30fps)
6. Click **Run workflow** to start
7. Download your video from artifacts when complete

#### Custom Props Examples

##### News Story Example

```json
{
  "backgroundImages": [
    {
      "url": "https://images.unsplash.com/photo-1504711434969-e33886168f5c",
      "animation": "zoom-in"
    }
  ],
  "textSegments": [
    {
      "text": "Breaking: New Innovation Launched",
      "startFrame": 30,
      "durationInFrames": 90,
      "animation": "fade"
    },
    {
      "text": "Revolutionary technology changes the game",
      "startFrame": 130,
      "durationInFrames": 90,
      "animation": "slide"
    }
  ],
  "publishDate": "2024-12-25",
  "tags": ["#Technology", "#Innovation", "#Breaking"],
  "copyright": "© 2024 Your Company. All rights reserved."
}
```

##### Quote Example

```json
{
  "quote": "The future belongs to those who believe in the beauty of their dreams.",
  "author": "Eleanor Roosevelt",
  "backgroundImage": {
    "url": "https://images.unsplash.com/photo-1506905925346-21bda4d32df4",
    "animation": "zoom-in"
  },
  "stories": ["Inspiration", "Dreams", "Motivation"]
}
```

#### Animation Options

**Background Animations:**
- `zoom-in` - Gradually zoom into image
- `zoom-out` - Gradually zoom out from image
- `fade` - Fade in and out
- `none` - No animation

**Text Animations:**
- `fade` - Smooth fade in/out
- `slide` - Slide in from bottom
- `typing` - Typewriter effect
- `none` - No animation

#### Duration Calculation

Duration is specified in frames at 30 fps:
- 30 frames = 1 second
- 300 frames = 10 seconds
- 600 frames = 20 seconds
- 900 frames = 30 seconds

## Workflow Features

### ✅ What the Workflows Do

1. **Checkout code** - Gets the latest code from your repository
2. **Setup Node.js** - Installs Node.js v20 with npm caching
3. **Install dependencies** - Runs `npm ci` to install all packages
4. **Ensure Chrome** - Downloads Chrome browser for rendering
5. **Render video** - Uses Remotion CLI to generate the video
6. **Upload artifact** - Saves the video as a GitHub artifact
7. **Output info** - Displays video details in the log

### 📦 Artifacts

Generated videos are uploaded as GitHub Actions artifacts:
- **Retention**: 30 days
- **Download**: From the workflow run page
- **Naming**: `generated-video-{CompositionID}` or `custom-video-{type}`

### ⚡ Performance

- **Average render time**: 1-3 minutes
- **Parallel execution**: You can run multiple workflows simultaneously
- **Caching**: npm dependencies are cached for faster subsequent runs

## Tips & Best Practices

### Image URLs

When using custom images:
- Use HTTPS URLs
- Ensure images are publicly accessible
- Recommended size: 1080x1920 (9:16 ratio) or larger
- Supported formats: JPG, PNG, WebP
- Use high-quality images for best results

### Text Content

For text segments:
- Keep text concise and readable
- Recommended: 2-3 lines per segment
- Test timing: 90-120 frames per segment works well
- Use animations to make text engaging

### Debugging

If a workflow fails:
1. Check the workflow logs for error messages
2. Verify JSON syntax in custom props
3. Ensure image URLs are accessible
4. Check that duration is sufficient for all content

### Local Testing

Before running in GitHub Actions, test locally:
```bash
npm install
npm run dev  # Preview in Remotion Studio
npm run build  # Test rendering
```

## Examples

### Example 1: Quick News Video

Use the **Generate Video** workflow:
- Composition: `BreakingNews`
- Filename: `breaking-news-2024.mp4`
- Quality: `18`

### Example 2: Custom Motivational Quote

Use the **Generate Custom Video** workflow:
- Type: `quote`
- Props: (see Quote Example above)
- Filename: `motivation-quote.mp4`
- Duration: `240` (8 seconds)

### Example 3: Tech News with Custom Images

Use the **Generate Custom Video** workflow:
- Type: `news`
- Props: (see News Story Example above)
- Filename: `tech-innovation.mp4`
- Duration: `300` (10 seconds)

## Integration with CI/CD

These workflows can be triggered programmatically using the GitHub API:

```bash
curl -X POST \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  https://api.github.com/repos/OWNER/REPO/actions/workflows/generate-video.yml/dispatches \
  -d '{"ref":"main","inputs":{"composition_id":"NewsStory","output_filename":"my-video.mp4"}}'
```

## Troubleshooting

### Common Issues

**Issue**: Workflow fails with "Image not found"
- **Solution**: Verify image URL is publicly accessible

**Issue**: Video is too short
- **Solution**: Increase `duration_frames` or adjust text segment timings

**Issue**: Text appears cut off
- **Solution**: Shorten text or split into multiple segments

**Issue**: JSON parsing error
- **Solution**: Validate JSON syntax using a JSON validator

### Getting Help

- Check the [main README](README.md) for general documentation
- Review [QUICKSTART.md](QUICKSTART.md) for basic usage
- See [FEATURES.md](FEATURES.md) for feature details
- Read [ADVANCED.md](ADVANCED.md) for customization options

## License

ISC
