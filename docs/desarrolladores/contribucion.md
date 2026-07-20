<!--markdownlint-disable MD007-->

# Contribución

## Ramas

- `dev`: rama de desarrollo, donde se integran los cambios.
- `main`: rama protegida. Sólo se puede modificar mediante **pull requests desde `dev`**.

## Convenciones de commits

Los mensajes de commit siguen un prefijo que indica el tipo de cambio:

| Prefijo | Uso |
| --- | --- |
| `feat:` | Nueva funcionalidad |
| `fix:` | Corrección de errores |
| `refactor:` | Cambios internos sin alterar el comportamiento |
| `style:` | Cambios de formato/estilo, sin efecto funcional |
| `chore:` | Mantenimiento (dependencias, configuración, etc.) |

## Hooks de pre-commit

El proyecto usa [`prek`](https://github.com/j178/prek) para forzar validaciones automáticas antes de confirmar o subir cambios. Debe instalarse tanto para `pre-commit` como para `pre-push`:

```sh
uv run prek install --hook-type pre-commit --hook-type pre-push
```

## Proceso de release

Un **pull request aprobado y fusionado a `main`** implica normalmente una nueva versión de la aplicación. Como parte de ese PR (o inmediatamente después), se actualiza manualmente [`versionamiento.md`](../versionamiento.md) con los cambios relevantes de la versión.
