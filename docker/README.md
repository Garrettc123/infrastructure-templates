# Docker Templates

## Base Images

### Language Bases
- [Python](base-images/python/)
- [Node.js](base-images/nodejs/)
- [Go](base-images/golang/)
- [Rust](base-images/rust/)

### Framework Bases
- [Django](base-images/django/)
- [FastAPI](base-images/fastapi/)
- [Express](base-images/express/)
- [React](base-images/react/)

## Applications

- [Web Servers](applications/web/)
- [API Services](applications/api/)
- [Workers](applications/workers/)
- [Cron Jobs](applications/cron/)

## Utilities

- [Build Tools](utilities/build/)
- [Testing](utilities/testing/)
- [Deployment](utilities/deployment/)

## Usage

```bash
# Build image
docker build -t app:latest -f docker/applications/web/Dockerfile .

# Run container
docker run -p 8000:8000 app:latest

# Push to registry
docker push garrettc123/app:latest
```