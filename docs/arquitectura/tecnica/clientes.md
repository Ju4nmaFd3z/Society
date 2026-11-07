# Clientes: Android y web

Cómo se organizan por dentro los dos clientes. Los dos solo hablan con la [API propia](api-propia.md) y siguen las [reglas de comunicación](comunicacion.md).

## Capa de red de Android

Sigue el patrón del proyecto de clase PokeDex2627, sin librerías añadidas:

| Pieza | Qué es |
|---|---|
| `NetworkModule` | Módulo de Hilt (`@InstallIn(SingletonComponent::class)`) que crea Retrofit con `GsonConverterFactory` y la URL de la API de `BuildConfig` |
| `SocietyApi` | Interfaz de Retrofit con funciones `suspend` que devuelven `Response<T>` |
| `*ApiModels.kt` | DTO (`data class`) escritos a mano a partir de `openapi.yaml`, con `toModel()` |
| `XxxRemoteDataSource` | Llama a `SocietyApi` y convierte la respuesta |
| `XxxRepository` + `XxxRepositoryImpl` | Lo que usan los ViewModel; se enlaza con `@Binds` en `AppModule` |
| OkHttp | Interceptor que añade `Authorization: Bearer` y `Authenticator` que renueva el token con `/auth/refresco` |

## Estructura de la app Android: una carpeta por pantalla

El código se organiza **por pantalla (feature)**, no por tipo de archivo. Cada pantalla tiene su carpeta en `feature/` y su ruta en `navigation/Rutas.kt`, así el `NavHost` se lee como el mapa de pantallas del [diagrama de flujo](../diagramas/diagrama-flujo-app.drawio) (página 1).

```
com.alanturing.society
├── MainActivity.kt          (intent-filter society://instagram)
├── core/
│   ├── network/             SocietyApi · NetworkModule · AuthInterceptor · AuthAuthenticator · ErrorMapper
│   ├── auth/                SesionRepository · TokenStore (DataStore)
│   ├── di/                  AppModule (@Binds de los repositorios)
│   └── model/               ErrorSociety
├── navigation/              Rutas.kt · SocietyNavHost.kt · ArranqueViewModel.kt
├── ui/
│   ├── theme/               Color · Theme · Type
│   └── component/           EstadoCargando · EstadoError · EstadoVacio · BotonPrincipal · TarjetaCristal
└── feature/
    ├── bienvenida/          S01
    ├── acceso/              S01A
    ├── empezamos/           S02
    ├── altamanual/          S02M
    ├── analisis/            S03
    ├── asiteveinternet/     S04
    ├── home/                S05
    ├── crearcontenidograph/ S06 · S07 · S08 (grafo anidado con un ViewModel compartido)
    ├── contenidocreado/     S09
    ├── publicarcontenido/   S09P
    ├── campanapropuesta/    S11
    ├── campanamedida/       S12 [P1]
    ├── perfil/              S13
    └── calendario/          S15 [P1]
```

Cada ruta lleva los datos que recibe su pantalla, según el diagrama de flujo: `AnalisisRoute(analisisId)`, `AsiTeVeInternetRoute(restauranteId, modoEdicion)`, `CrearContenidoGraph(campanaId)` (opcional, solo desde S11), `ContenidoCreadoRoute(piezaId)`, `PublicarContenidoRoute(piezaId)` y `CampanaPropuestaRoute(oportunidadId)`. El resto no recibe datos.

Dentro de cada `feature/<pantalla>/`:

| Carpeta | Qué va |
|---|---|
| `XxxNavigation.kt` | `navigateToXxx()` para llegar a la pantalla y `xxxDestination()` con lo que se muestra; los botones son callbacks y el destino lo decide `SocietyNavHost` |
| `ui/` | `XxxScreen` (Compose), `XxxViewModel` con `StateFlow<XxxUiState>` y `ui/component/` para piezas propias de esa pantalla |
| `data/` | `XxxRemoteDataSource` y `XxxRepository` + `Impl`; los DTO de Gson en `data/dto/` con su `toModel()` |
| `model/` | Clases que usa la interfaz (nunca los DTO) |

Regla: cada pantalla nueva crea su carpeta con estas tres partes y su entrada en `Rutas.kt`. El panel Angular usa los mismos nombres en `web/src/app/features/`.
