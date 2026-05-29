# Event Companion Pack Feed Starter

This folder is a starter package for the separate dev event-pack feed repo.

Copy the contents of this folder into the new GitHub repo, not the other way around.

Suggested repo name:

```text
event-companion-pack-feed-dev
```

This feed is temporary field-test infrastructure. It is not the final production backend.

## Structure

```text
catalog.json
manifests/
  dev-e17.json
packs/
  dev/
    e17-compact-walk-test/
      e17-compact-walk-test__2026-05-29T120000Z.json
schemas/
  manifest.schema.json
  pack.schema.json
```

## App Manifest URL

Once the repo exists, the app can use a fixed dev manifest URL:

```text
https://raw.githubusercontent.com/<github-user-or-org>/event-companion-pack-feed-dev/main/manifests/dev-e17.json
```

Do not use a private repo for this first dev feed unless we explicitly add authentication.

## Naming Rules

Use stable pack IDs:

```text
e17-compact-walk-test
e17-wide-cycle-test
walthamstow-core
north-east-london-core
```

Use immutable versioned pack filenames:

```text
e17-compact-walk-test__2026-05-29T120000Z.json
```

Avoid:

```text
today.json
tomorrow.json
latest.json
current.json
```

The manifest points to the currently active immutable pack file.

## Update Flow

For field testing, an update script should:

```text
1. Generate a new timestamped pack file.
2. Update manifests/dev-e17.json to point at the new pack file.
3. Update catalog.json if a new manifest or coverage cluster is added.
4. Commit and push.
```

The app should check the manifest on its two-hour refresh target and download packs only when the manifest points to a changed version.

## Timing Rule

Production event packs must contain real source times.

Development-only rolling time belongs in local fixtures or in a separate generator script before publishing the pack. Do not publish fields such as:

```text
dev_start_offset_minutes
dev_duration_minutes
```

inside production-shaped event packs.

## Current Compatibility

The sample files use schema version `0.1`, matching the current app-side `LocalJSONEventPackRepository` decoder.

Extra metadata fields such as `version`, `kind`, and `coverage_hint` are included for future compatibility. The current app decoder can ignore unknown fields safely.
