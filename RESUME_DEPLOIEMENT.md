# Resume du deploiement BoviBot

## 1. Ou l'application a ete deployee
- Plateforme: AWS EC2
- Systeme: Ubuntu
- Instance: t3.micro
- Application backend: FastAPI (Uvicorn)
- Verification locale serveur: `/health` repond correctement

## 2. Ou la base de donnees a ete deployee
- Plateforme: AWS RDS
- Moteur: MySQL 8.4
- Base: bovibot
- Schema SQL importe avec succes (`schema.sql`)
- Objets verifies:
  - Tables
  - Fonctions
  - Triggers
  - Events

## 3. Nom de domaine configure
- Domaine utilise: `bovibot.mouhadev.com`
- Le sous-domaine pointe vers l'IP publique de l'instance EC2
- Type DNS utilise: enregistrement A (pas CNAME vers une IP)

## 4. Configuration Nginx
- Nginx installe sur l'instance EC2
- Role: reverse proxy
- Redirection:
  - Entrant: HTTP/HTTPS sur le domaine
  - Sortant interne: `127.0.0.1:8002` (Uvicorn)
- Objectif: acceder a l'application sans `:8002`

## 5. Etat actuel
- Deploiement application: OK
- Deploiement base de donnees: OK
- Domaine: OK
- Nginx reverse proxy: OK

## 6. Service de demarrage automatique (systemd)

Fichier service cree sur EC2:
- /etc/systemd/system/bovibot.service

Contenu du service:

```ini
[Unit]
Description=BoviBot FastAPI
After=network.target

[Service]
User=ubuntu
Group=ubuntu
WorkingDirectory=/home/ubuntu/BoviBOT
EnvironmentFile=/home/ubuntu/BoviBOT/.env
ExecStart=/home/ubuntu/BoviBOT/venv/bin/uvicorn app:app --host 127.0.0.1 --port 8002
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Commandes utilisees:

```bash
sudo systemctl daemon-reload
sudo systemctl enable bovibot
sudo systemctl start bovibot
sudo systemctl status bovibot
```

--- 

