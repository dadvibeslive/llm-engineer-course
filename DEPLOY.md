# LLM Engineer Course — deploy

- Live: https://llm-engineer-course.pages.dev/
- GitHub: https://github.com/dadvibeslive/llm-engineer-course
- Cloudflare Pages project: `llm-engineer-course`, production branch `main`, account `164cc96a3405f726ec67269826ce6fcf`
- Source of truth: this directory; `index.html` is the export of the Muse artifact `llm-engineer-self-learning-course`

## Update flow

1. Edit the course in Muse, re-export the artifact, replace `index.html` here.
2. Commit and push to GitHub `main`.
3. Deploy to Cloudflare (wrangler 4.160+ does NOT auto-create a Pages project — the project already exists, so deploy directly):

```bash
cd ~/workspace/llm-engineer-course && python3 -c "
import sys, os, subprocess
sys.path.insert(0, '/opt/muse/skills/custom-connector-setup/bin')
from dynamic_credentials import dynamic_credential_entry
surr = dynamic_credential_entry('custom.cloudflare')['surrogate']
env = dict(os.environ, CLOUDFLARE_API_TOKEN=surr, CLOUDFLARE_ACCOUNT_ID='164cc96a3405f726ec67269826ce6fcf')
subprocess.run(['npx', '-y', 'wrangler', 'pages', 'deploy', '.', '--project-name=llm-engineer-course', '--branch=main'], env=env, check=True)
"
```

Never put a raw token in a command, file, or log — the `hsurr:*` surrogate is swapped device-side.

## Verify

`curl -s -o /dev/null -w "%{http_code}" https://llm-engineer-course.pages.dev/` → expect 200, title "LLM Engineer Self-Learning Course".
