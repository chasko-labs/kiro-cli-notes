# Woodpecker CI Migration

## migrated workflows

| workflow | status | notes |
| -------- | ------ | ----- |
| `health-check.yml` | ready | validates knowledge.json every 6 hours |
| `deploy-pages.yml` | requires setup | github pages deploy step blocked until woodpecker-agent-github-pages integration ready |
| `update-knowledge-base.yml` | ready | auto-updates knowledge base on docs changes, commits to main |

## blocked: github pages deployment

the original `deploy.yml` uses `actions/upload-pages-artifact@v3` + `actions/deploy-pages@v4`. these are github-native actions with no woodpecker equivalent.

two paths forward:

1. manual setup — configure github pages to pull from main branch directly (no ci deploy step). knowledge base artifact is already committed to main by `update-knowledge-base.yml`
2. woodpecker-agent-github-pages — create dedicated agent that runs `gh pages deploy` via api (future)

recommended: path 1 for now. pages will auto-publish from main branch after workflow succeeds.

## local testing

```bash
# validate yaml syntax
woodpecker-cli lint .woodpecker/*.yml

# dry-run against rocm woodpecker instance
export WOODPECKER_SERVER=https://woodpecker.rocm-aibox
export WOODPECKER_TOKEN=<token>
woodpecker-cli exec .woodpecker/health-check.yml
```
