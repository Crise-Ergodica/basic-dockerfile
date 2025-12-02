# 🐳 Basic Dockerfile Project

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Docker](https://img.shields.io/badge/Docker-Alpine-blue.svg)](https://www.docker.com/)
[![Platform](https://img.shields.io/badge/Platform-Linux-orange.svg)](https://www.kernel.org/)

Projeto prático para aprender **Docker** do zero! Cria imagens Docker simples que imprimem mensagens no console.

> 🗺️ **Projeto baseado em**: [roadmap.sh - Basic Dockerfile](https://roadmap.sh/projects/basic-dockerfile)

## 🎯 Objetivo

Aprender os fundamentos de Docker criando um `Dockerfile` que:

- ✅ Usa `alpine:latest` como base (imagem ultra-leve!)
- ✅ Imprime "Hello, Captain!" no console
- ✅ Encerra automaticamente após imprimir
- ✅ Versão avançada: aceita seu nome como argumento

## 💻 O que é Docker? (Explicação Simples)

### **Sem Docker (Problema)**

```bash
# Sua máquina:
Python 3.8, Node 14, Ubuntu 20.04

# Máquina do colega:
Python 3.10, Node 18, Ubuntu 22.04

# Servidor de produção:
Python 3.9, Node 16, CentOS 7

😱 "Funciona na minha máquina!"
```

### **Com Docker (Solução)**

```bash
# Qualquer máquina:
Docker roda o MESMO container

✅ Mesmas dependências
✅ Mesmo sistema operacional
✅ Mesmo ambiente

🎉 "Funciona em TODAS as máquinas!"
```

**Docker = Caixa mágica que empacota sua aplicação + dependências**

## 📦 Estrutura do Projeto

```
basic-dockerfile/
├── Dockerfile              # Versão básica
├── Dockerfile.advanced     # Versão com argumento customizável
└── README.md               # Este arquivo
```

## 🚀 Instalação do Docker

### **Ubuntu/Debian**

```bash
# Instalar Docker
sudo apt-get update
sudo apt-get install docker.io -y

# Adicionar seu usuário ao grupo docker (evita sudo)
sudo usermod -aG docker $USER

# Relogar ou executar:
newgrp docker

# Verificar instalação
docker --version
# Docker version 24.0.7, build afdd53b
```

### **Testar Docker**

```bash
docker run hello-world

# Se funcionar, você verá:
# "Hello from Docker!"
# "This message shows that your installation appears to be working correctly."
```

## 📝 Dockerfile Básico (Versão 1)

### **Arquivo: `Dockerfile`**

```dockerfile
# Imagem base - Alpine Linux (super leve!)
FROM alpine:latest

# Comando que será executado quando o container iniciar
CMD ["echo", "Hello, Captain!"]
```

### **Explicação Linha por Linha**

```dockerfile
FROM alpine:latest
```
- **FROM** = Imagem base (ponto de partida)
- **alpine:latest** = Distribuição Linux ultra-leve (~5MB!)
- **:latest** = Versão mais recente

```dockerfile
CMD ["echo", "Hello, Captain!"]
```
- **CMD** = Comando executado quando container inicia
- **["echo", "..."]** = Formato JSON (recomendado)
- **echo** = Comando Linux para imprimir texto

### **Como Usar**

```bash
# 1. Clonar o repositório
git clone https://github.com/Crise-Ergodica/basic-dockerfile.git
cd basic-dockerfile

# 2. Construir a imagem
docker build -t hello-captain .

# 3. Executar o container
docker run hello-captain

# Output:
# Hello, Captain!
```

### **Explicação dos Comandos**

```bash
docker build -t hello-captain .
```
- **build** = Construir imagem
- **-t hello-captain** = Tag/nome da imagem
- **.** = Contexto (diretório atual)

```bash
docker run hello-captain
```
- **run** = Executar container
- **hello-captain** = Nome da imagem

## 🚀 Dockerfile Avançado (Versão 2)

### **Arquivo: `Dockerfile.advanced`**

```dockerfile
# Imagem base - Alpine Linux
FROM alpine:latest

# Argumento de build com valor padrão
ARG NAME=Captain

# Variável de ambiente a partir do argumento
ENV GREETING_NAME=${NAME}

# Comando que usa a variável
CMD echo "Hello, ${GREETING_NAME}!"
```

### **Explicação das Novas Instruções**

```dockerfile
ARG NAME=Captain
```
- **ARG** = Argumento de build (usado APENAS durante construção)
- **NAME=Captain** = Valor padrão se não passar argumento

```dockerfile
ENV GREETING_NAME=${NAME}
```
- **ENV** = Variável de ambiente (disponível no container)
- **${NAME}** = Usa valor do ARG

### **Como Usar**

#### **Versão Padrão (Captain)**

```bash
docker build -f Dockerfile.advanced -t hello-custom .
docker run hello-custom

# Output:
# Hello, Captain!
```

#### **Versão Personalizada (Seu Nome)**

```bash
# Com seu nome
docker build -f Dockerfile.advanced -t hello-aurora --build-arg NAME=Aurora .
docker run hello-aurora

# Output:
# Hello, Aurora!

# Ou qualquer nome:
docker build -f Dockerfile.advanced -t hello-joao --build-arg NAME="João" .
docker run hello-joao

# Output:
# Hello, João!
```

### **Explicação dos Novos Parâmetros**

```bash
-f Dockerfile.advanced
```
- **-f** = File (especificar qual Dockerfile usar)
- Por padrão Docker procura arquivo chamado "Dockerfile"

```bash
--build-arg NAME=Aurora
```
- **--build-arg** = Passar argumento para ARG no Dockerfile
- **NAME=Aurora** = Define valor do ARG NAME

## 📊 Comandos Docker Essenciais

### **Gerenciar Imagens**

```bash
# Listar todas as imagens
docker images

# Remover imagem
docker rmi hello-captain

# Remover imagens não usadas
docker image prune
```

### **Gerenciar Containers**

```bash
# Listar containers rodando
docker ps

# Listar TODOS os containers (inclusive parados)
docker ps -a

# Remover container
docker rm <container-id>

# Remover todos os containers parados
docker container prune
```

### **Inspecionar**

```bash
# Ver histórico de construção
docker history hello-captain

# Ver detalhes da imagem
docker inspect hello-captain

# Ver logs do container
docker logs <container-id>
```

## 🔍 Conceitos-Chave do Docker

### **1. Imagem vs Container**

```bash
IMAGEM = Receita de bolo (estática)
CONTAINER = Bolo assado (executável)

# Uma imagem pode gerar vários containers:
docker run hello-captain  # Container 1
docker run hello-captain  # Container 2
docker run hello-captain  # Container 3
```

### **2. Layers (Camadas)**

```dockerfile
FROM alpine:latest    # Layer 1 (base)
ARG NAME=Captain      # Layer 2 (argumento)
ENV GREETING_NAME     # Layer 3 (env)
CMD echo ...          # Layer 4 (comando)
```

**Vantagem**: Docker reutiliza layers que não mudaram = builds mais rápidos!

### **3. FROM vs CMD vs RUN**

```dockerfile
FROM alpine:latest           # Imagem base
RUN apk add --no-cache curl  # Executa DURANTE build
CMD ["curl", "google.com"]   # Executa QUANDO container inicia
```

## 🎓 Exercícios Práticos

### **Exercício 1: Modificar Mensagem**

```dockerfile
# Modifique para imprimir "Olá, Brasil!"
FROM alpine:latest
CMD ["echo", "Olá, Brasil!"]
```

### **Exercício 2: Múltiplas Mensagens**

```dockerfile
# Imprimir 2 mensagens
FROM alpine:latest
CMD echo "Linha 1" && echo "Linha 2"
```

### **Exercício 3: Usar Variáveis de Ambiente**

```dockerfile
FROM alpine:latest
ENV MESSAGE="Mensagem customizada"
CMD echo "$MESSAGE"
```

## 🐛 Troubleshooting

### **Erro: "Cannot connect to the Docker daemon"**

```bash
# Iniciar serviço Docker
sudo systemctl start docker

# Verificar status
sudo systemctl status docker
```

### **Erro: "permission denied"**

```bash
# Adicionar usuário ao grupo docker
sudo usermod -aG docker $USER

# Relogar ou:
newgrp docker
```

### **Imagem não encontrada**

```bash
# Verificar imagens disponíveis
docker images

# Rebuild se necessário
docker build -t hello-captain .
```

## 🚀 Próximos Passos

### **Nível 2: Aplicativo Web**

```dockerfile
FROM python:alpine
WORKDIR /app
COPY app.py .
CMD ["python", "app.py"]
```

### **Nível 3: Multi-Stage Build**

```dockerfile
# Stage 1: Build
FROM golang:alpine AS builder
COPY . .
RUN go build -o app

# Stage 2: Runtime
FROM alpine:latest
COPY --from=builder /app /app
CMD ["/app"]
```

### **Nível 4: Docker Compose**

```yaml
version: '3'
services:
  app:
    build: .
    ports:
      - "8080:8080"
  db:
    image: postgres:alpine
```

## 📚 Recursos Adicionais

- [Docker Documentation](https://docs.docker.com/)
- [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)
- [Docker Hub](https://hub.docker.com/) - Repositório de imagens
- [Alpine Linux](https://alpinelinux.org/) - Documentação da base
- [roadmap.sh - DevOps Roadmap](https://roadmap.sh/devops)

## 📝 Licença

MIT License - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 👤 Autor

**Aurora Ergodica**
- GitHub: [@Crise-Ergodica](https://github.com/Crise-Ergodica)
- Email: gdcm10@gmail.com
- Portfólio: [DevOps Roadmap](https://github.com/Crise-Ergodica/DevOps-roadmap)

## 🔗 Projetos Relacionados

- [📊 Linux Server Stats](https://github.com/Crise-Ergodica/Linux-server-stats)
- [📈 Nginx Log Analyser](https://github.com/Crise-Ergodica/nginx-log-analyser)
- [📦 Log Archive Tool](https://github.com/Crise-Ergodica/log-archive-tool)
- [🚀 GitHub Actions Deployment](https://github.com/Crise-Ergodica/gh-deployment-workflow)

---

<div align="center">

**🐳 Docker simplificado!**

**Clone, build, run e aprenda!**

*"God's in His heaven, all's right with the world!"*

Feito com ❤️ por [Aurora Ergodica](https://github.com/Crise-Ergodica)

</div>
