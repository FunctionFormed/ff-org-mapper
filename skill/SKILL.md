---
name: ff-org-mapper
description: >-
  Maps organisations described in conversation into a JSON file for the
  ff-org-mapper HTML app. Use when the user wants an org map, org JSON,
  stakeholder map, or to import people and teams into ff-org-mapper.
---

# ff-org-mapper

Extract organisation data from the conversation and deliver a JSON file the user can import into the local HTML app. Do not fetch or deliver the HTML app.

App: https://github.com/FunctionFormed/ff-org-mapper

## Output

Deliver only JSON. Filename: `{org-slug}-org-YYYY-MM-DD.json` (today's date). If the org has no name: `organisation-org-YYYY-MM-DD.json`.

Do not fetch `ff-org-mapper.html` from GitHub. Do not inline or attach the HTML.

## Connections — never generate

Always seed `"connections": []`.

Never infer reporting lines, "reports to", manager, sponsor, or any other relationship as a connection. Connections are personal and contextual. The user draws them by hand on the Connections & Groups canvas.

Remove all connection-inference logic. Even if the conversation says "Adam reports to Susie", do not add a connection object.

`groupLinks` is also always `[]` unless the user explicitly asked you to encode a group-to-group link.

## Groups and canvas

- Seed named `groups` as empty containers (id, name, color).
- Every person has `"groups": []`. Do not pre-assign people to groups.
- `"layout"` is always `{ "nodes": {}, "groups": {} }`.

## Schema

Top level:

```json
{ "orgs": [ /* one or more orgs */ ] }
```

Each org:

```json
{
  "id": "org-1",
  "name": "Organisation",
  "color": "#2196f3",
  "tags": [],
  "roleTags": [],
  "roleOrder": [],
  "groups": [],
  "campaigns": [],
  "connections": [],
  "groupLinks": [],
  "layout": { "nodes": {}, "groups": {} },
  "people": []
}
```

Person:

```json
{
  "id": "person-name",
  "name": "Name",
  "role": "",
  "side": "int",
  "email": "",
  "notes": "",
  "tags": [],
  "groups": [],
  "status": "available",
  "away": ""
}
```

`side`: `"int"` (user's org), `"client-side"` (client contacts), `"other"`.

Each person's `tags` must be a subset of the parent org's `tags`.

Group:

```json
{
  "id": "grp-slug",
  "name": "Name",
  "color": ["#e3f2fd", "#1565c0"]
}
```

Storage key in the app is `teamdir-v3`. Do not include it in the JSON file.

## After delivery

Tell the user:

1. Download the app from https://github.com/FunctionFormed/ff-org-mapper if they do not have it.
2. Open `ff-org-mapper.html` directly in the browser (not inside Claude chat).
3. Import the JSON from the sidebar.
4. Merge adds new orgs and skips existing IDs. Replace overwrites everything in this browser.
5. Drag people onto the Connections & Groups canvas to place them. Draw connections there yourself — none are included in the file.
