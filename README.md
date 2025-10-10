# Backstage Catalog Demo

Demo catalog entities for Backstage with TechDocs and ADRs.

## Contents

- **Platform System**: Core platform grouping
- **Payment Service**: Payment processing microservice with ADRs
- **User API**: User management and authentication service with ADRs
- **Frontend App**: React-based customer-facing application

## Structure

```
catalog/
├── platform-system.yaml           # System entity
├── payment-service/
│   ├── catalog-info.yaml         # Component definition
│   ├── mkdocs.yml                # TechDocs config
│   └── docs/
│       ├── *.md                  # Documentation
│       └── adr/                  # Architecture Decision Records
├── user-api/
│   ├── catalog-info.yaml
│   ├── mkdocs.yml
│   └── docs/
│       ├── *.md
│       └── adr/
└── frontend-app/
    ├── catalog-info.yaml
    ├── mkdocs.yml
    └── docs/
        └── *.md
```

## Usage in Backstage

Add to your `app-config.yaml`:

```yaml
catalog:
  locations:
    - type: url
      target: https://github.com/wingleung/backstage-catalog-demo/blob/main/catalog/platform-system.yaml
    - type: url
      target: https://github.com/wingleung/backstage-catalog-demo/blob/main/catalog/payment-service/catalog-info.yaml
    - type: url
      target: https://github.com/wingleung/backstage-catalog-demo/blob/main/catalog/user-api/catalog-info.yaml
    - type: url
      target: https://github.com/wingleung/backstage-catalog-demo/blob/main/catalog/frontend-app/catalog-info.yaml
```
