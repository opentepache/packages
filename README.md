# OpenTepache packages

Recetas del repositorio APT [OpenTepache](https://apt.opentepache.org): herramientas de
desarrollo recientes empaquetadas como `.deb` firmados para Debian (`bookworm`, `trixie`, `sid`)
y Ubuntu (`noble`, `resolute`), en `amd64` y `arm64`.

**¿Quieres instalar los paquetes?** Sigue la [guía del usuario](docs/usuario.md).

## Qué hay aquí

| Ruta | Qué es |
|---|---|
| `packages/<p>.yaml` | manifiesto de cada paquete: versión, artefactos de upstream, licencia, archivos |
| `packages/<p>.lock.yaml` | sha256 verificados de cada artefacto (lockfile) |
| `config/debfactory.yaml` | configuración del repositorio: suites, arquitecturas, firma, repositorios comunitarios conocidos |
| `config/licenses.yaml` | licencias aceptadas |
| `repo/state.yaml`, `repo/audit.jsonl` | qué está publicado y el registro de cada publicación (los escribe el pipeline) |
| `debfactory.version` | versión de la herramienta que construye y publica |
| `.github/workflows/` | el pipeline: `pr.yml` prueba cada PR y `main.yml` publica |

Cada cambio integrado en `main` se construye, pasa lintian, se prueba instalándolo en un
contenedor limpio de cada suite y arquitectura, y solo entonces se firma y se publica. Cada
`.deb` publicado tiene una entrada en `repo/audit.jsonl` con el commit que lo produjo.

## Contribuir

1. **Antes de proponer un paquete**, comprueba que aporta algo:
   - su autor no publica un repositorio APT oficial;
   - la versión en Debian estable no está al día con la última versión estable de upstream;
   - ningún repositorio comunitario de `config/debfactory.yaml` (`community_repos`) lo
     distribuye **gratis, sin registro y al día**. Los repositorios de pago o con suscripción
     no cuentan.
2. **Solo versiones estables**: nada de alpha, beta, rc, preview, nightly, tip, canary, dev ni
   releases marcadas como pre-release por upstream.
3. Abre un PR con el manifiesto (`packages/<p>.yaml`), explicando en `value` por qué aporta valor.
4. **Lockfile**: la herramienta que lo genera todavía no es pública. El check del PR fallará en
   la coherencia del lockfile; el mantenedor lo genera y lo sube a la rama de tu PR (deja
   marcado *Allow edits by maintainers*).
5. El código de un PR de un fork se ejecuta solo cuando el mantenedor lo aprueba, sin acceso a
   la llave de firma ni a credenciales.

Las reglas completas están en la constitución del proyecto; las de arriba son las que afectan a
una contribución.

## Licencia

Las recetas y la configuración de este repositorio están bajo licencia [MIT](LICENSE). Cada
paquete conserva la licencia de su upstream.
