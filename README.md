# DevOps Portfolio — TP07: CI/CD

![CI/CD Pipeline](https://github.com/TU_USUARIO/devops-TP06/actions/workflows/cicd.yml/badge.svg)

App de notas con pipeline CI/CD completo usando GitHub Actions.

## Pipeline

|   Stage    |       Trigger   | Qué hace                           |
|------------|-----------------|-------------------------------------|
| lint       | todo push       | flake8 en Python, yamllint en YAML |
| test       | después de lint | pytest con reporte de cobertura    |
| build-push | main y develop  | docker buildx, push a Docker Hub   |
| deploy     | solo main       | SSH al servidor, compose pull + up |

## 4 Secrets requeridos

- DOCKERHUB_USERNAME
- DOCKERHUB_TOKEN
- DEPLOY_HOST
- DEPLOY_USER
- DEPLOY_SSH_KEY

## Correr tests localmente

```bash
cd backend
pip install -r requirements.txt
pytest tests/ -v --cov=. --cov-report=term-missing
```

## ⚠️ Nota sobre el step de Deploy

El job `deploy` del pipeline realiza un deploy via SSH a un servidor externo.
Para reproducirlo se requiere:

- Un servidor Linux con Docker y Docker Compose instalados
- Acceso SSH con usuario `root` (o equivalente con permisos Docker)
- El directorio `/opt/devops-portfolio/` con el `docker-compose.yml` presente
- Los siguientes secrets configurados en el repo:

| Secret | Descripción |
|--------|-------------|
| `DEPLOY_HOST` | IP o dominio del servidor de deploy |
| `DEPLOY_USER` | Usuario SSH del servidor |
| `DEPLOY_SSH_KEY` | Clave privada SSH (contenido completo del archivo) |

> Durante la entrega del TP07 el deploy se ejecutó sobre un entorno
> efímero de [KillerCoda](https://killercoda.com) (Ubuntu playground).
