# Portainer Agent (Standard) — VM remota

Este diretório contém o `docker-compose` para rodar o Portainer Agent (modo Standard) em uma VM remota.

Local: `/opt/projects/portainer-agent`

## Arquivos

- `docker-compose.yml` — define o service `portainer-agent` (porta 9001).

## Quando usar

- Use o modo Standard quando o agente estiver em uma VM na mesma rede ou quando você puder abrir a porta `9001` no agente.
- Se o agente estiver atrás de NAT/firewall sem portas de entrada, prefira **Edge Agent** (não coberto aqui).

## Requisitos na VM do agent

- Docker instalado
- Porta TCP `9001` aberta/encaminhada para a VM (inbound) para que o Portainer server consiga conectar

### Exemplo (Debian/Ubuntu) para abrir porta 9001 com `ufw`

```bash
sudo ufw allow 9001/tcp
sudo ufw reload
```

## Como iniciar

No host da VM do agente:

```bash
cd /opt/projects/portainer-agent
docker compose up -d
```

Verifique com:

```bash
docker ps --filter name=portainer-agent
curl -sS http://127.0.0.1:9001 || true
```

## Adicionar endpoint no Portainer (Server)

1. No Portainer (servidor), vá em "Environment-related" → "Add environment".
2. Escolha "Agent" e informe `tcp://<IP-da-VM-do-agent>:9001`.
3. Salve — o Portainer deve conectar e listar o endpoint.

## Observações

- O volume `/var/run/docker.sock` é montado dentro do container do agent; portanto o agente expõe a interface para gerenciar o Docker host. Proteja a VM e só instale agentes em hosts confiáveis.
- Se preferir, posso gerar também um `systemd` unit para garantir start automático fora do compose.
