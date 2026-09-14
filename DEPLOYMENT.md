# Deployment plan

The source repository is initialized and the development branch is `dev/traffic-rewrite`.

## Large asset constraint
The current city model is roughly 189 MB, which is above GitHub's normal 100 MB per-file limit. For that reason the stable QA deployment should upload the complete static build directly to Netlify (or use Git LFS / a dedicated asset host later).

## Target workflow
1. GitHub stores source, configuration, issues, and development history.
2. Netlify hosts the full static build including large GLB assets.
3. Every tested build keeps a stable QA URL.
4. Traffic and police work happens on `dev/traffic-rewrite` and is merged only after the QA scenario passes.

## QA scenario
Spawn -> enter stadium pitch -> security approaches -> warnings -> police cars arrive -> officers approach -> dialogue -> flee by car -> police vehicle pursuit.
