# Clientes: Android y web

Cómo se organizan por dentro los dos clientes. Los dos solo hablan con la [API propia](api-propia.md) y siguen las [reglas de comunicación](comunicacion.md).

## Capa de red de Android

Sigue el patrón del proyecto de clase PokeDex2627, sin librerías añadidas:

| Pieza | Qué es |
|---|---|
| `NetworkModule` | Módulo de Hilt (`@InstallIn(SingletonComponent::class)`) que crea el cliente OkHttp, con el `AuthInterceptor`, y Retrofit con `GsonConverterFactory` y la URL fija de `API_BASE` |
| `SocietyApi` | Interfaz de Retrofit con funciones `suspend` que devuelven `Response<T>` |
| `*ApiModels.kt` | DTO (`data class`) escritos a mano a partir de `openapi.yaml`, con `toModel()` |
| `XxxRemoteDataSource` | Llama a `SocietyApi` y convierte la respuesta |
| `XxxRepository` + `XxxRepositoryImpl` | Lo que usan los ViewModel; se enlaza con `@Binds` en `AppModule` |
| `DataStoreModule` | Módulo de Hilt que crea el DataStore donde se guarda la sesión |
| OkHttp | `AuthInterceptor`, que añade `Authorization: Bearer` y, ante un 401, renueva una vez con `/auth/refresco` y repite la llamada |

## Estructura de la app Android: una carpeta por pantalla

El código se organiza **por pantalla (feature)**, no por tipo de archivo. Cada pantalla tiene su carpeta en `feature/` y su ruta en `navigation/Rutas.kt`, así el `NavHost` se lee como el mapa de pantallas del [diagrama de flujo](../diagramas/diagrama-flujo-app.drawio) (página 1).

```
com.alanturing.society
├── MainActivity.kt          (intent-filter society://instagram)
├── core/
│   ├── network/             SocietyApi · AuthInterceptor · ErrorMapper
│   ├── di/                  AppModule (@Binds de los repositorios) · NetworkModule · DataStoreModule
│   └── model/               ErrorSociety
├── navigation/              Rutas.kt · SocietyNavHost.kt
├── ui/
│   ├── theme/               Color · Theme · Type
│   └── component/           EstadoCargando · EstadoError · EstadoVacio · BotonPrincipal · TarjetaCristal
└── feature/
    ├── arranque/            ArranqueViewModel · Destino (no es pantalla)
    ├── bienvenida/          S01
    ├── acceso/              S01A · sesión en data/local (DataStore)
    ├── empezamos/           S02
    ├── altamanual/          S02M
    ├── analisis/            S03
    ├── ficha/               S04
    ├── home/                S05
    ├── crearcontenido/      S06 · S07 · S08 (grafo anidado con un ViewModel compartido)
    ├── contenido/           S09 · S09P
    ├── campanas/            S11 · S12 [P1]
    ├── perfil/              S13
    └── calendario/          S15 [P1]
```

Dentro de cada `feature/<pantalla>/`:

| Carpeta | Qué va |
|---|---|
| `ui/` | `XxxScreen` (Compose), `XxxViewModel` con `StateFlow<XxxUiState>` y `ui/component/` para piezas propias de esa pantalla |
| `data/` | `XxxRemoteDataSource` y `XxxRepository` + `Impl`; los DTO de Gson en `data/dto/` con su `toModel()` |
| `model/` | Clases que usa la interfaz (nunca los DTO) |

Regla: cada pantalla nueva crea su carpeta con estas tres partes y su entrada en `Rutas.kt`. El panel Angular usa los mismos nombres en `web/src/app/features/`.
