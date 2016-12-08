# Manual de parametrização da App do Sonar no Azure Active Directory

## Parametrizações no `Manifest.json` pelo novo portal (https://portal.azure.com)

Adicionar os seguintes objetos no array `appRoles`: 

```json
{
  "allowedMemberTypes": [
    "User"
  ],
  "displayName": "Access Sonar as normal user",
  "id": "058cc1e3-d054-474a-b429-656654b5795c",
  "isEnabled": true,
  "description": "Allow User level access to Sonar",
  "value": "User"
},
{
  "allowedMemberTypes": [
    "User"
  ],
  "displayName": "Access Sonar as admin user",
  "id": "fad35625-2c52-4382-a73c-60c9d4afe6ed",
  "isEnabled": true,
  "description": "Allow Admin level access to Sonar",
  "value": "Admin"
}
```

## Parametrizações na aplicação pelo portal clássico (https://manage.windowsazure.com)

Configurações que devem ser feitas na aplicação a partir do portal clássico.

Abaixo seguem as parametrizações separadas por seção de acordo com o portal de configuração.

### Properties

- Name: **Sonar Core Antifraude**
- Sign-on Url: **{Url do Sonar em Produção}**
- Application is Multitenant: **No**
- User Assignment Required to Access App: **Yes**


### Keys

Gerar uma `Key` com duração de **1 Ano**

### Single Sign-On

- App Id URI: **{Url do Sonar em Produção}/Sonar**
- Reply URL:  
  1. **{Url do Sonar em Produção}**
  2. **{Url do Sonar em Homologação}** *Temporario apenas para validação*

### Permission to Other Applications

- Windows Azure Active Directory:
  - Delegated Permissions: 
    - **Sign in and read user profile**
    - **Read directory data**
