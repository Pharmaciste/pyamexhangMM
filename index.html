// api/anchor.js
export default async function handler(req, res) {
  try {
    const {
      ADMIN_PASSWORD,
      GITHUB_TOKEN,
      GITHUB_OWNER,
      GITHUB_REPO,
      GITHUB_BRANCH = 'main',
    } = process.env;

    if (!GITHUB_TOKEN || !GITHUB_OWNER || !GITHUB_REPO) {
      res.status(500).send('Server not configured');
      return;
    }

    const ghHeaders = {
      'Authorization': `token ${GITHUB_TOKEN}`,
      'Accept': 'application/vnd.github+json',
      'User-Agent': 'cztimer-site',
    };
    const filePath = 'anchor.json';
    const ghContentUrl = `https://api.github.com/repos/${GITHUB_OWNER}/${GITHUB_REPO}/contents/${filePath}?ref=${GITHUB_BRANCH}`;

    async function readCurrent() {
      const r = await fetch(ghContentUrl, { headers: ghHeaders });
      if (r.status === 404) {
        return { cfg: { anchor_iso: '2025-09-07T16:53:00Z', manual_offset_seconds: 0, paused: false }, sha: null };
      }
      if (!r.ok) throw new Error('GitHub read failed: ' + r.status);
      const j = await r.json();
      const content = Buffer.from(j.content, 'base64').toString('utf8');
      return { cfg: JSON.parse(content), sha: j.sha };
    }

    async function writeConfig(newCfg, prevSha) {
      const body = {
        message: `Update anchor.json ${new Date().toISOString()}`,
        content: Buffer.from(JSON.stringify(newCfg, null, 2)).toString('base64'),
        branch: GITHUB_BRANCH,
      };
      if (prevSha) body.sha = prevSha;
      const r = await fetch(`https://api.github.com/repos/${GITHUB_OWNER}/${GITHUB_REPO}/contents/${filePath}`, {
        method: 'PUT',
        headers: { ...ghHeaders, 'Content-Type': 'application/json' },
        body: JSON.stringify(body),
      });
      if (!r.ok) {
        const t = await r.text().catch(()=> '');
        throw new Error('GitHub write failed: ' + r.status + ' ' + t);
      }
      return newCfg;
    }

    if (req.method === 'GET') {
      const { cfg } = await readCurrent();
      res.setHeader('Content-Type', 'application/json; charset=utf-8');
      res.setHeader('Cache-Control', 'no-store');
      res.status(200).send(JSON.stringify(cfg));
      return;
    }

    if (req.method === 'POST') {
      const body = await readBody(req);
      if (!body || body.password !== ADMIN_PASSWORD) {
        res.status(401).send('Unauthorized');
        return;
      }

      const { anchor_iso, manual_offset_seconds = 0, paused = false } = body;
      if (!anchor_iso || Number.isNaN(Date.parse(anchor_iso))) {
        res.status(400).send('Invalid anchor_iso');
        return;
      }
      if (typeof manual_offset_seconds !== 'number') {
        res.status(400).send('manual_offset_seconds must be number');
        return;
      }
      if (typeof paused !== 'boolean') {
        res.status(400).send('paused must be boolean');
        return;
      }

      const { sha } = await readCurrent();
      const saved = await writeConfig({ anchor_iso, manual_offset_seconds, paused }, sha);

      res.setHeader('Content-Type', 'application/json; charset=utf-8');
      res.status(200).send(JSON.stringify(saved));
      return;
    }

    res.setHeader('Allow', 'GET, POST');
    res.status(405).send('Method Not Allowed');
  } catch (e) {
    res.status(500).send(String(e?.message || e));
  }
}

function readBody(req) {
  return new Promise((resolve) => {
    let data = '';
    req.on('data', (c) => (data += c));
    req.on('end', () => {
      try { resolve(JSON.parse(data || '{}')); }
      catch { resolve(null); }
    });
  });
}
