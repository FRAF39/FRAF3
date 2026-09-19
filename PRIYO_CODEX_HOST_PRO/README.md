# PRIYO_CODEX HOST — Professional Edition

A polished self-hosting control panel for projects, uploads and deployments.

## What was upgraded

- Professional responsive dashboard UI
- Light / dark mode with saved preference
- Workspace navigation: Overview, Projects, Deployments, File Manager, Settings
- Project cards with runtime badges, upload and deploy actions
- Project search
- Deployment activity table with status indicators
- Deployment log viewer
- File manager with project selector and file metadata
- Guided project creation modal with advanced build/start commands
- Account and runtime-access settings
- Improved admin console with dedicated user creation view
- User enable/disable, password reset and deletion controls
- Mobile-friendly responsive layout
- No change to the existing PostgreSQL authentication/data model
- Existing HTML, Node.js and Python runtime permissions are preserved

## Render deployment

The included `Dockerfile` and `render.yaml` are already structured for Render.

1. Push this folder to a GitHub repository.
2. In Render, create a Blueprint from that repository.
3. Set `ADMIN_USERNAME` and `ADMIN_PASSWORD`.
4. Set `PUBLIC_BASE_URL` to the final Render URL.
5. Deploy.

### Important architecture note

Render web services do not expose a Docker daemon to application code. The existing deployment engine therefore expects `DOCKER_HOST` to point to a separate, isolated Docker executor when you want the panel to build/run arbitrary Node/Python/HTML project containers.

The panel itself can run on Render without that executor. If `DOCKER_HOST` is not configured, project deployment attempts will fail with a clear executor-configuration message rather than exposing a privileged Docker socket.

For production, keep the Docker executor isolated and never mount a privileged host Docker socket into a public web service.

## Local development

```bash
docker compose up --build
```

Open `http://localhost:8080`.

Default local admin credentials are configured in `docker-compose.yml` and should be changed for any non-local use.

## Production checklist

- Use a strong `SESSION_SECRET`
- Use strong admin credentials
- Set `PUBLIC_BASE_URL`
- Use a dedicated isolated Docker executor for arbitrary project deployments
- Consider external/object storage for user uploads because standard Render web-service filesystems are not a durable project-storage layer
- Put the application behind HTTPS
