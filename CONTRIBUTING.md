# Contribuciones

## Acerca del Proyecto

Este es un proyecto educativo estático desarrollado como parte del curso de Ambiente y Sustentabilidad de la Universidad Autónoma de Nuevo León. El sitio web enseña los principios fundamentales que rigen la naturaleza y los ecosistemas de nuestro planeta.

## Arquitectura Técnica

### Tecnologías del Lado del Cliente

- **HTML5**: Estructura semántica del sitio con múltiples páginas
- **Tailwind CSS** (vía CDN): Framework de estilos utilitario
- **CSS Personalizado**: Estilos adicionales y animaciones en `styles/main.css`
- **JavaScript (ES6+)**: Funcionalidad interactiva del foro en `JS/main.js`
- **Bootstrap Icons**: Biblioteca de iconos
- **HTML5 Video**: Video de fondo en la página principal

### Servidor embebido

- **Nginx** (Alpine): Servidor web ligero que sirve los archivos estáticos
- Puerto expuesto: **8007**

## Estructura del Proyecto

```
WebAmbiente/
├── index.html                    # Página principal
├── README.md                     # Documentación del proyecto
├── CONTRIBUTING.md               # Guía técnica y contribuciones
├── Dockerfile                    # Definición de la imagen Docker
├── docker-compose.yml            # Orquestación de servicios
├── nginx.conf                    # Configuración de Nginx
├── EQ3_PRINCIPIOS-BASICOS-DE-LA-NATURALEZA.pptx
├── images/                       # Recursos de imagen
├── videos/                       # Videos (naturaleza.mp4)
├── pages/                        # Páginas secundarias
│   ├── blog.html
│   ├── entrada1.html - entrada5.html
│   ├── foro.html
│   └── equipo.html
├── styles/
│   └── main.css
└── JS/
    └── main.js
```

## Dockerización

### Imagen Base

```dockerfile
FROM nginx:alpine
```

Se utiliza `nginx:alpine` por ser una imagen ligera (~15MB) ideal para servir contenido estático.

### Configuración de Nginx

El archivo `nginx.conf` configura el servidor para:
- Escuchar en el puerto 80
- Servir archivos desde `/usr/share/nginx/html`
- Usar `index.html` como índice
- Manejar rutas con `try_files` para permitir navegación SPA-like

### Docker Compose

```yaml
services:
  web:
    build: .
    container_name: naturaleza
    ports:
      - "8007:80"
    restart: always
```

- **build**: Construye la imagen desde el Dockerfile en el directorio actual
- **container_name**: Asigna el nombre "naturaleza" al contenedor
- **ports**: Mapea el puerto 8007 del host al puerto 80 del contenedor
- **restart: always**: Reinicia el contenedor automáticamente si falla

## Ejecutar el Proyecto

### Opción 1: Docker Compose (Recomendado)

```bash
# Iniciar el contenedor en modo demonio
docker-compose up -d

# Acceder al sitio
# http://localhost:8007

# Ver logs en tiempo real
docker-compose logs -f

# Detener servicios
docker-compose down
```

### Opción 2: Docker Manual

```bash
# Construir la imagen
docker build -t naturaleza-web .

# Ejecutar el contenedor
docker run -p 8007:80 --name naturaleza naturaleza-web

# Acceder al sitio
# http://localhost:8007
```

### Opción 3: Sin Docker

Simplemente abre `index.html` directamente en un navegador web.

## Desarrollo

### Modificar Estilos

Los estilos personalizados se encuentran en `styles/main.css`. Este archivo complementa Tailwind CSS con:
- Animaciones personalizadas
- Estilos específicos del tema
- Efectos visuales adicionales

### Modificar JavaScript

La funcionalidad del foro está en `JS/main.js`:
- Creación de publicaciones con imagen (localStorage)
- Sistema de "me gusta"
- Sistema de comentarios
- Navegación responsiva

### Agregar Contenido

1. **Nuevas entradas del blog**: Agregar archivos HTML en `pages/`
2. **Imágenes**: Agregar archivos en `images/`
3. **Videos**: Agregar archivos en `videos/`

## Contribuir

1. Haz un fork del repositorio
2. Crea una rama para tu funcionalidad (`git checkout -b feature/nueva-funcionalidad`)
3. Realiza tus cambios
4. Ejecuta el contenedor para verificar
5. Envía un pull request

## Notas Importantes

- El foro utiliza `localStorage` del navegador para persistir datos
- No se requiere base de datos - todo funciona en el cliente
- El video de fondo debe estar en formato MP4 para compatibilidad