# Multi Environment Application Deployment

## Overview

I deployed a ticket management app with dev and prod environments using Docker Compose on AWS EC2. The app has 3 services - frontend, dev backend and prod backend, plus MongoDB for each environment.

## My Setup

- AWS EC2, Ubuntu 24.04, t3.small instance
- Region: us-east-1
- Docker and Docker Compose installed

## Security Group

Opened these ports:
- 22 (SSH)
- 3000 (frontend)
- 3001 (dev backend)
- 3002 (prod backend)
All with source 0.0.0.0/0

## Steps

1. Launched EC2 instance with Ubuntu 24.04 and t3.small
2. Opened ports 22, 3000, 3001, 3002 in security group
3. Installed Docker and Git
4. Cloned the repo
5. Created Dockerfiles for frontend, dev backend and prod backend
6. Created docker-compose.yml with 5 services (frontend, backend-dev, backend-prod, mongo-dev, mongo-prod)
7. Set up .env files for both backends
8. Built and ran: docker compose build && docker compose up -d

## Dockerfiles

backend/dev/Dockerfile - Python 3.10, port 3001
backend/prod/Dockerfile - Python 3.10, port 3002  
frontend/Dockerfile - Node 18, port 3000

## docker-compose.yml

5 services: frontend, backend-dev, backend-prod, mongo-dev, mongo-prod
All on a bridge network (multienv-net)

## Environment Variables

Dev:
MONGO_URI=mongodb://mongo-dev:27017/devdb
PORT=3001
FLASK_ENV=development

Prod:
MONGO_URI=mongodb://mongo-prod:27017/proddb
PORT=3002
FLASK_ENV=production

## Access URLs

Frontend: http://EC2_PUBLIC_IP:3000
Dev: http://EC2_PUBLIC_IP:3000/dev
Prod: http://EC2_PUBLIC_IP:3000/prod

## Testing

All 5 containers were running when I checked with docker ps.
Opened the URLs in browser and they worked.

## Screenshots

I have taken the following screenshots:

1. docker-ps.png - Output of docker ps showing all 5 containers running
2. frontend-home.png - Frontend page at port 3000
3. dev-page.png - /dev route showing development environment
4. prod-page.png - /prod route showing production environment
5. ec2-instance.png - EC2 instance details from AWS Console
6. security-group.png - Inbound rules in security group

## Issues Faced

1. t2.micro instance was too small, containers kept crashing. Switched to t3.small and it worked.
2. Had to open ports 3000, 3001, 3002 in security group to access from browser.
3. Flask needed to bind to 0.0.0.0 for container networking to work.

## Cleanup

docker compose down
