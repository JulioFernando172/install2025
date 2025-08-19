# Compatibilidade com Ubuntu 22.04 LTS

Este documento descreve as modificações feitas no instalador do Whaticket para garantir compatibilidade com Ubuntu 22.04 LTS.

## Principais Correções Implementadas

### 1. Verificação de Versão do Sistema
- Adicionada função `system_check_ubuntu_version()` que verifica se o sistema é Ubuntu 20.04 ou superior
- O instalador agora exibe a versão detectada e interrompe a execução em versões não suportadas

### 2. Instalação do Docker
**Problema:** O repositório 'bionic' não é adequado para Ubuntu 22.04
**Solução:**
- Substituído repositório 'bionic' por 'jammy' (Ubuntu 22.04)
- Atualizado método de adição de chaves GPG (apt-key está deprecated)
- Implementado método moderno usando `/etc/apt/keyrings/`
- Adicionados plugins Docker Compose e BuildX

### 3. Instalação do PostgreSQL
**Problema:** Comando `apt-key` está deprecated no Ubuntu 22.04
**Solução:**
- Substituído `apt-key add` por `gpg --dearmor`
- Chaves GPG agora são armazenadas em `/etc/apt/trusted.gpg.d/`

### 4. Dependências do Puppeteer
**Problema:** Alguns pacotes mudaram de nome ou foram removidos no Ubuntu 22.04
**Solução:**
- `libgcc1` → `libgcc-s1`
- `libappindicator1` → `libappindicator3-1`
- Removido `gconf-service` e `libgconf-2-4` (obsoletos)
- Adicionados novos pacotes: `libdrm2`, `libxkbcommon0`, `libatspi2.0-0`

### 5. Atualização do Sistema
**Melhorias:**
- Adicionado `apt upgrade` para garantir que todos os pacotes estejam atualizados
- Instalação de ferramentas essenciais: `build-essential`, `python3`, `python3-pip`
- Melhor organização dos pacotes instalados

## Como Usar

### Pré-requisitos
- Ubuntu 20.04 LTS ou superior (recomendado: Ubuntu 22.04 LTS)
- Acesso root ou sudo
- Conexão com a internet

### Instalação Primária
```bash
cd /caminho/para/instalador
sudo chmod +x install_primaria
sudo ./install_primaria
```

### Instalação de Instância
```bash
cd /caminho/para/instalador
sudo chmod +x install_instancia
sudo ./install_instancia
```

## Versões Testadas
- ✅ Ubuntu 22.04 LTS (Jammy Jellyfish)
- ✅ Ubuntu 20.04 LTS (Focal Fossa)
- ❌ Ubuntu 18.04 LTS (Bionic Beaver) - Não suportado

## Notas Importantes

1. **Backup:** Sempre faça backup do seu sistema antes de executar o instalador
2. **Permissões:** O script requer privilégios de root para instalar pacotes do sistema
3. **Firewall:** Certifique-se de que as portas necessárias estejam abertas
4. **DNS:** Verifique se os domínios estão configurados corretamente

## Solução de Problemas

### Erro de Chave GPG
Se encontrar erros relacionados a chaves GPG:
```bash
sudo apt-key del ACCC4CF8  # Remove chave antiga do PostgreSQL
sudo rm -f /etc/apt/trusted.gpg.d/postgresql.gpg
# Execute o instalador novamente
```

### Erro de Repositório Docker
Se o Docker não instalar corretamente:
```bash
sudo rm -f /etc/apt/sources.list.d/docker.list
sudo rm -f /etc/apt/keyrings/docker.gpg
# Execute o instalador novamente
```

### Verificar Logs
Para verificar logs de instalação:
```bash
sudo journalctl -u nginx
sudo journalctl -u postgresql
sudo docker logs redis-[nome_instancia]
```

## Suporte

Em caso de problemas:
1. Verifique se está usando Ubuntu 22.04 LTS
2. Certifique-se de que todos os pré-requisitos foram atendidos
3. Consulte os logs do sistema para erros específicos
4. Verifique a conectividade com a internet

---

**Última atualização:** $(date)
**Versão do instalador:** 2.0 (Ubuntu 22.04 Compatible)