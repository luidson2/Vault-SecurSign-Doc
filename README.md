# Vault SecurSign

Vault SecurSign é uma task para Azure Pipelines que assina automaticamente arquivos JAR, EXE, DLL, entre outros, utilizando certificados armazenados com segurança no Azure Key Vault, seja por conexão ARM ou autenticação manual via Service Principal. A task detecta o tipo de arquivo automaticamente e utiliza o assinador apropriado.

## Funcionalidades

* Integração com o Azure Key Vault para uso seguro de certificados
* Suporte a autenticação via Service Connection ou manual com credenciais de App Registration
* Detecção automática de tipo de binário
* Suporte multiplataforma (Windows/Linux)
* Uso de timestamp opcional
* Testado com arquivos `.jar`, `.exe`, `.dll`, `.msi`, `.py`, entre outros
* Suporte a curingas no `filePath` (ex: `caminho/*.jar`, `**/*.dll`)

## Métodos de Autenticação

| Método | Descrição |
|--------|-----------|
| ARM - Service Connection | Usa uma conexão existente configurada no Azure DevOps com acesso ao Key Vault |
| Manual | Usa os valores diretos de `Client ID`, `Client Secret` e `Tenant ID` |


## Requisitos do build agent

* JDK instalado (recomendado versão 21 ou 23) com `jarsigner` disponível no `PATH`
* .NET SDK instalado e disponível no `PATH`
* O certificado no Key Vault deve estar configurado para operações de assinatura (sign)
* Acesso à internet para baixar dependências
* Compatível com agentes Microsoft-hosted e self-hosted (Windows ou Linux)

## Entradas da Task

| Campo | Descrição |
|-------|-----------|
| `authMode` | Método de autenticação: `serviceConnection` ou `manual` |
| `connection` | Conexão ARM com acesso ao Key Vault |
| `clientId`, `clientSecret`, `tenantId` | Dados manuais de autenticação (visíveis se `authMode = manual`) |
| `keyVaultName` | Nome do Key Vault no Azure |
| `certificateName`      | Nome (alias) do certificado dentro do Key Vault |
| `filePath`             | Caminho ou padrão dos arquivos a assinar (ex: `pasta/*.jar`) |
| `timestampUrl` | URL opcional para serviço de timestamp |


## Exemplo de Uso

```yaml
- task: VaultSecurSign@2
  inputs:
    authMode: 'serviceConnection'
    connection: 'MinhaConexaoAzure'
    keyVaultName: 'meu-keyvault'
    certificateName: 'cert-assinatura'
    filePath: '$(Build.ArtifactStagingDirectory)/binarios/*.jar'
    timestampUrl: 'http://timestamp.globalsign.com/tsa/advanced'
```

## Autor
**Luídson Santos**  - [LinkedIn](https://www.linkedin.com/in/luidson-santos) | [GitHub](https://github.com/luidson2)