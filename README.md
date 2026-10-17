# Laboratorio 02 — Infraestructura como Código (Grupo 5)

Despliegue de un contenedor Docker local usando dos herramientas de IaC —
**Terraform** (provider `kreuzwerker/docker`) y **Pulumi** (Python + `pulumi-docker`) —
para compararlas y analizar el *drift* de configuración.

## Integrantes y roles

| Nombre | Rol | Responsabilidad principal |
|--------|-----|---------------------------|
| Camilo Alfonso Mosquera Benavides  | R1 | Terraform (`terraform/`) |
| Jonathan David Moncaleano Angel | R2 | Pulumi (`pulumi/`) |
| Andrés Felipe Lugo Rodríguez | R3 | Documentación y CI (`docs/`, `.github/`) |

## Estructura del repositorio

```
lab02-iac-grupo5/
├── README.md
├── .gitignore
├── .github/workflows/iac-ci.yml   # Pipeline Shift-Left para validar PRs
├── terraform/                     # R1
├── pulumi/                        # R2
└── docs/informe_comparativo.md    # R3
```

## Requisitos previos

- Docker Engine en ejecución (`docker info`)
- Terraform >= 1.5
- Pulumi CLI >= 3.100
- Python >= 3.10

## Despliegue local

### Terraform

```bash
cd terraform
cp terraform.tfvars.example terraform.tfvars   # ajustar valores
terraform init
terraform fmt -check
terraform validate
terraform plan
terraform apply
# Limpieza
terraform destroy
```

### Pulumi

```bash
cd pulumi
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
pulumi login --local
pulumi stack select dev --create
pulumi preview
pulumi up
# Limpieza
pulumi destroy
```

## Integración continua

Cada Pull Request hacia `main` ejecuta `.github/workflows/iac-ci.yml`, que valida
el formato y la sintaxis de Terraform y del código Pulumi antes de permitir el merge.
