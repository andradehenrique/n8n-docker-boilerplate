# N8N Docker Boilerplate

A simple, production-ready boilerplate for running N8N with Docker Compose and PostgreSQL.

## Features

- 🐳 Docker Compose setup with PostgreSQL
- 🔒 Secure database configuration
- 📁 File sharing support
- ⚙️ Environment-based configuration
- 🚀 One-command deployment

## Quick Start

1. **Clone this repository**
   ```bash
   git clone git@github.com:andradehenrique/n8n-docker-boilerplate.git
   cd n8n-docker-boilerplate
   ```

2. **Setup environment**
   ```bash
   cp .env.example .env
   # Edit .env with your settings
   ```

3. **Create Docker network**
   ```bash
   docker network create ai-docker-network
   ```

4. **Start services**
   ```bash
   docker compose up -d
   ```

5. **Access N8N**
   
   Open http://localhost:5678 in your browser

## Prerequisites

- Docker and Docker Compose
- 2GB+ RAM available
- Port 5678 available (or configure another)

## Project Structure

```
.
├── docker-compose.yml    # Main services configuration
├── init-data.sh         # PostgreSQL initialization script
├── .env.example         # Environment variables template
├── local-files/         # Shared files directory
└── README.md           # This file
```

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `POSTGRES_USER` | PostgreSQL admin user | `postgres` |
| `POSTGRES_PASSWORD` | PostgreSQL admin password | - |
| `POSTGRES_DB` | Database name | `n8n` |
| `POSTGRES_NON_ROOT_USER` | N8N database user | `n8n_user` |
| `POSTGRES_NON_ROOT_PASSWORD` | N8N database password | - |
| `N8N_WEBHOOK_URL` | Webhook base URL | - |

## Available Scripts

```bash
# Start services
npm run up

# Stop services  
npm run down

# View logs
npm run logs

# Check status
npm run status

# Clean everything (⚠️ removes volumes)
npm run clean
```

## Tunnel for Webhook Development

Para desenvolvimento local com webhooks acessíveis pela internet, você pode usar um tunnel. Isso permite que ferramentas externas trigem seus workflows do N8N mesmo rodando localmente.

### Usando Pinggy.io

O Pinggy.io oferece tunnels gratuitos e seguros. Para criar um tunnel para seu N8N local:

```bash
ssh -p 443 -o StrictHostKeyChecking=no -o ServerAliveInterval=30 -R0:localhost:5678 a.pinggy.io
```

Isso criará um tunnel que:
- 🌐 Expõe seu N8N local (porta 5678) na internet
- 🔗 Fornece uma URL pública temporária
- 🔒 Mantém a conexão segura via SSH
- ⚡ Permite desenvolvimento em tempo real

**Exemplo de uso:**
1. Inicie seu N8N local: `docker compose up -d`
2. Execute o comando do tunnel em outro terminal
3. Use a URL fornecida pelo Pinggy como webhook URL
4. Configure seus workflows para receber webhooks externos

### Outras opções de tunnel

- **Ngrok**: `ngrok http 5678`
- **LocalTunnel**: `lt --port 5678`
- **Serveo**: `ssh -R 80:localhost:5678 serveo.net`

> ⚠️ **Atenção**: Use tunnels apenas para desenvolvimento. Para produção, configure um domínio próprio e SSL.

## Production Configuration

For production deployments:

1. Configure environment variables in `.env`
2. Update all passwords and secrets
3. Configure SSL/HTTPS
4. Set up proper domain and webhooks

## Networking

This setup uses an external Docker network (`ai-docker-network`). To disable:

Remove these lines from `docker-compose.yml`:
```yaml
networks:
  - ai-external-network

# And the network definition at the bottom
```

## Troubleshooting

### Port Already in Use
```bash
# Change port in docker-compose.yml
ports:
  - "8080:5678"  # Use port 8080 instead
```

### Database Connection Issues
- Check environment variables
- Wait for PostgreSQL health check
- Check logs: `docker compose logs postgres`

### Permission Issues
```bash
sudo chown -R 1000:1000 local-files/
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Links

- [N8N Documentation](https://docs.n8n.io/)
- [N8N Community](https://community.n8n.io/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
