# Drupal AI: Focal Point Demo

Demo repository that configures the **AI Automator** with the new **Focal Point** operation type. Use this to automate subject detection on image uploads so focal point fields get set by a Vision Language Model (e.g. GPT-4o, Llava) instead of manual cropping.

## What This Demo Covers

- **Focal Point operation type** — Standardized operation in the Drupal AI module so VLMs can return subject coordinates and the AI Automator updates focal point fields. No more custom glue code.
- **Streaming fix** — The API Explorer and provider plugins now handle the `stream` parameter consistently so you can debug long chains without a blank screen.

## Requirements

- Drupal 10 or 11 with the [AI (Artificial Intelligence) module](https://www.drupal.org/project/ai)
- [Focal Point](https://www.drupal.org/project/focal_point) (or any module that provides a focal point field)
- Patches from the Drupal.org issues below (until they land in the AI module)

## Setup

1. **Install the AI and Focal Point modules**

   ```bash
   composer require drupal/ai drupal/focal_point
   drush en ai focal_point -y
   ```

2. **Apply the Focal Point operation type patch** (issue [#3571939](https://www.drupal.org/project/ai/issues/3571939))

   The patch adds a `FocalPoint` operation type so the AI Automator can map VLM output (subject coordinates) to your entity’s focal point field. Apply the patch from the issue, then clear cache.

3. **Apply the streaming fix** (issue [#3571925](https://www.drupal.org/project/ai/issues/3571925)) if you use the API Explorer

   This fixes output buffering so chunked responses stream correctly in the admin UI.

4. **Configure an AI Automator** for focal point

   - In the AI module, create an Automator that triggers on your image entity/field (e.g. media image upload).
   - Set the operation type to **Focal Point** and map the Vision model’s response to the focal point field.
   - Flow: Image uploaded → AI Automator runs → Vision model analyzes image → Returns subject coords → Focal Point operation updates the field → Content saved.

## Flow (conceptual)

```
Image Uploaded → AI Automator → Vision Model Request → Identify Subject Coords
       → Focal Point Operation → Update Entity Focal Point Field → Content Published
```

## References

- [Add Focal Point Operation Type, API Explorer and LLM Provider](https://www.drupal.org/project/ai/issues/3571939)
- [Output streaming is handled inconsistently, fails to work in API AI-explorer](https://www.drupal.org/project/ai/issues/3571925)

## License

MIT
