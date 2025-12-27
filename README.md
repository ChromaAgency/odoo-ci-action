# Odoo CI Action

Una acción de GitHub Actions para ejecutar tests de módulos Odoo y reportar cobertura de código a Code Climate.

## 🎯 ¿Qué hace esta acción?

Esta acción automatiza el proceso de testing de módulos Odoo en tu pipeline de CI/CD:

- ✅ Ejecuta tests de módulos Odoo en un entorno aislado con Docker
- 🐘 Configura automáticamente PostgreSQL como base de datos
- 🔐 Soporta módulos de Odoo Enterprise mediante SSH deploy keys
- 📊 Genera reportes de cobertura de código
- 🚀 Integración directa con Code Climate (Qlty) para análisis de calidad

## 📦 Uso

### Ejemplo Básico

```yaml
name: Odoo Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Run Odoo Tests
        uses: ChromaAgency/odoo-ci-action@main
        with:
          odoo_version: '16.0'
          modules: 'my_custom_module'
```

### Ejemplo con Enterprise y Code Climate

```yaml
name: Odoo Tests with Coverage

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Run Odoo Tests
        uses: ChromaAgency/odoo-ci-action@main
        with:
          odoo_version: '17.0'
          modules: 'my_module_1,my_module_2'
          pg_version: '15'
          test_tags: '/module_name'
          enterprise_deploy_key: ${{ secrets.ODOO_ENTERPRISE_DEPLOY_KEY }}
          qlty_coverage_token: ${{ secrets.QLTY_COVERAGE_TOKEN }}
```

## ⚙️ Inputs

| Input | Descripción | Requerido | Default |
|-------|-------------|-----------|---------|
| `odoo_version` | Versión de Odoo a utilizar (ej: 16.0, 17.0) | ✅ | `16.0` |
| `modules` | Lista de módulos a probar separados por comas | ✅ | - |
| `pg_version` | Versión de PostgreSQL | ❌ | `14` |
| `test_tags` | Tags de test opcionales para filtrar tests específicos | ❌ | `''` |
| `enterprise_deploy_key` | SSH deploy key con acceso a odoo/enterprise | ❌ | `''` |
| `qlty_coverage_token` | Token de Code Climate/Qlty para reportar cobertura | ❌ | `''` |

## 🔑 Configuración de Secrets

### Para módulos Enterprise

1. Genera una SSH deploy key con acceso de lectura al repositorio de Odoo Enterprise
2. Añade la clave privada como secret en tu repositorio: `ODOO_ENTERPRISE_DEPLOY_KEY`

### Para Code Climate

1. Obtén tu token de Qlty/Code Climate
2. Añádelo como secret: `QLTY_COVERAGE_TOKEN`

## 🏗️ Arquitectura

La acción realiza los siguientes pasos:

1. **Infraestructura**: Crea una red Docker y levanta PostgreSQL
2. **Clonado Enterprise** (opcional): Clona el repositorio de Odoo Enterprise si se proporciona la deploy key
3. **Ejecución de Tests**: Ejecuta los tests en un contenedor de Odoo
4. **Cobertura** (opcional): Genera reporte de cobertura si se proporciona el token de Code Climate
5. **Reporte**: Sube el reporte a Code Climate para análisis

## 🤝 Cómo Contribuir

¡Las contribuciones son bienvenidas! Aquí te explicamos cómo puedes colaborar:

### 1. Fork y Clone

```bash
# Fork el repositorio desde GitHub, luego:
git clone https://github.com/TU_USUARIO/odoo-ci-action.git
cd odoo-ci-action
```

### 2. Crea una rama para tu feature

```bash
git checkout -b feature/mi-nueva-caracteristica
```

### 3. Realiza tus cambios

Edita el archivo `action.yml` según necesites. Asegúrate de:

- Mantener la compatibilidad con versiones anteriores
- Documentar nuevos inputs en este README
- Probar tus cambios en un repositorio de prueba

### 4. Prueba tus cambios

Crea un workflow de prueba en `.github/workflows/test.yml`:

```yaml
name: Test Action

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ./
        with:
          odoo_version: '16.0'
          modules: 'base'
```

### 5. Commit y Push

```bash
git add .
git commit -m "feat: descripción de tu cambio"
git push origin feature/mi-nueva-caracteristica
```

### 6. Abre un Pull Request

Ve a GitHub y abre un Pull Request desde tu rama hacia `main`. Describe:

- ❓ Qué problema resuelve
- 🔧 Qué cambios realizaste
- ✅ Cómo lo probaste

## 📝 Convenciones de Commits

Seguimos [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` Nueva funcionalidad
- `fix:` Corrección de bugs
- `docs:` Cambios en documentación
- `refactor:` Refactorización de código
- `test:` Añadir o mejorar tests
- `chore:` Tareas de mantenimiento

## 🐛 Reportar Issues

Si encuentras un bug o tienes una sugerencia:

1. Revisa si ya existe un issue similar
2. Abre un nuevo issue con:
   - Descripción clara del problema
   - Pasos para reproducirlo
   - Versión de Odoo y configuración utilizada
   - Logs relevantes

## 📄 Licencia

MIT License - ver el archivo LICENSE para más detalles.

## 👥 Autores

Mantenido por [ChromaAgency](https://github.com/ChromaAgency)

---

**¿Preguntas?** Abre un issue y estaremos encantados de ayudarte.
