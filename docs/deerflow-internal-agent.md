# DeerFlow internal agent integration

This branch turns Palmier Pro into the editor shell and treats DeerFlow as the internal production agent.

Palmier Pro should keep its MCP server available at `http://127.0.0.1:19789/mcp` so tools can still inspect and modify the timeline.

DeerFlow should replace Claude, Cursor, or Codex as the planning brain for Full Production.

The app should send DeerFlow the project id, creator goal, video metadata, transcript, timeline state, and edit constraints.

DeerFlow should return a structured edit decision plan.

Palmier Pro should execute the plan on the timeline.

DeerFlow should not appear as a visible product inside the UI.

The user should only see the normal Palmier Pro editing experience, plus one Full Production flow if that feature is enabled.

## Ownership of responsibilities

Palmier Pro owns the macOS app, timeline, media library, undo, preview, and final timeline state.

The MCP server owns timeline/tool access.

DeerFlow owns planning, reasoning, sub-agent orchestration, quality review, and structured edit decisions.

OpenMontage standards own the taste layer: hook, pacing, silence removal, zooms, captions, B-roll suggestions, meme beats, and final review.

FFmpeg or the existing render pipeline owns heavy media processing.

## Full Production contract

Request shape:

```json
{
  "projectId": "string",
  "creatorGoal": "string",
  "videoMetadata": {
    "durationSeconds": 0,
    "frameRate": 0,
    "width": 0,
    "height": 0,
    "hasAudio": true
  },
  "transcript": "string",
  "timelineState": "string",
  "constraints": [
    "keep Palmier Pro as the editor",
    "keep DeerFlow UI-hidden",
    "return structured decisions only"
  ]
}
```

Response shape:

```json
{
  "summary": "string",
  "hook": [],
  "cuts": [],
  "pacing": [],
  "captions": [],
  "broll": [],
  "qualityChecks": []
}
```

Each decision should include an action, optional start time, optional end time, reason, and confidence.

## Target behavior

Full Production should feel like one button.

Palmier Pro should ask DeerFlow for the plan.

Palmier Pro should apply the returned decisions.

The user should be able to undo the edit.

No Claude, Cursor, or Codex should be required at runtime.

External MCP clients can remain supported for advanced users.
