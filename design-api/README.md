# API Design and Documentation Workshop

```
openapi: 3.0.0
info:
  title: API de Comercio Electrónico
  description: API para gestionar registro e inicio de sesión de usuarios, carritos, productos, órdenes y reseñas de clientes en un sistema de comercio electrónico.
  version: 1.0.0
servers:
  - url: https://api.example.com/v1
tags:
  - name: Autenticación
    description: Operaciones de registro e inicio de sesión de usuarios
  - name: Carrito
    description: Operaciones relacionadas con el carrito de compra
  - name: Productos
    description: Operaciones relacionadas con productos en el catálogo
  - name: Órdenes
    description: Operaciones relacionadas con órdenes de compra
  - name: Reseñas
    description: Operaciones relacionadas con las reseñas de clientes
paths:
  /auth/register:
    post:
      summary: Registro de nuevo usuario
      description: Registra un nuevo usuario en el sistema.
      tags:
        - Autenticación
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Register'
      responses:
        '200':
          description: OK. Usuario registrado exitosamente.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          description: Solicitud incorrecta. La entrada no es válida.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '500':
          description: Error interno del servidor. No se pudo registrar el usuario.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
  /auth/login:
    post:
      summary: Inicio de sesión de usuario
      description: Inicia sesión de un usuario en el sistema.
      tags:
        - Autenticación
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Credentials'
      responses:
        '200':
          description: OK. Sesión iniciada exitosamente.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Session'
        '401':
          description: No autorizado. Las credenciales proporcionadas son incorrectas.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '500':
          description: Error interno del servidor. No se pudo iniciar sesión.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
  /cart:
    get:
      summary: Obtener el carrito del cliente
      description: Recupera el carrito asociado al cliente.
      tags:
        - Carrito
      parameters:
        - name: Authorization
          in: header
          description: Token JWT de autorización.
          required: true
          schema:
            type: string
      responses:
        '200':
          description: OK. Lista de carritos recuperada exitosamente.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Cart'
    post:
      summary: Crear un nuevo carrito
      description: Crea un nuevo carrito.
      tags:
        - Carrito
      parameters:
        - name: Authorization
          in: header
          description: Token JWT de autorización.
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CartRequest'
      responses:
        '200':
          description: OK. Carrito creado exitosamente.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Cart'
        '400':
          description: Solicitud incorrecta. La entrada no es válida.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '500':
          description: Error interno del servidor. No se pudo crear el carrito.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
  /product:
    get:
      summary: Obtener todos los productos
      description: Recupera una lista de todos los productos en el catálogo.
      tags:
        - Productos
      parameters:
        - name: Authorization
          in: header
          description: Token JWT de autorización.
          required: true
          schema:
            type: string
      responses:
        '200':
          description: OK. Lista de productos recuperada exitosamente.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Product'
    post:
      summary: Crear un nuevo producto
      description: Añade un nuevo producto al catálogo.
      tags:
        - Productos
      parameters:
        - name: Authorization
          in: header
          description: Token JWT de autorización.
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ProductRequest'
      responses:
        '200':
          description: OK. Producto creado exitosamente.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Product'
        '400':
          description: Solicitud incorrecta. La entrada no es válida.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '500':
          description: Error interno del servidor. No se pudo crear el producto.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
  /order:
    post:
      summary: Crear nueva orden
      description: Crea una nueva orden basada en el contenido actual del carrito del usuario.
      tags:
        - Órdenes
      parameters:
        - name: Authorization
          in: header
          description: Token JWT de autorización.
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/OrderRequest'
      responses:
        '200':
          description: OK. Orden creada exitosamente.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
        '400':
          description: Solicitud incorrecta. La entrada no es válida.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '500':
          description: Error interno del servidor. No se pudo crear la orden.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
  /review:
    post:
      summary: Agregar reseña
      description: Agrega una reseña para un producto específico.
      tags:
        - Reseñas
      parameters:
        - name: Authorization
          in: header
          description: Token JWT de autorización.
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ReviewRequest'
      responses:
        '200':
          description: OK. Reseña agregada exitosamente.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Review'
        '400':
          description: Solicitud incorrecta. La entrada no es válida.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '500':
          description: Error interno del servidor. No se pudo agregar la reseña.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
          example: 5
      required:
        - nombre
        - email
    Register:
      type: object
      properties:
        name:
          type: string
          example: Ricardo
        email:
          type: string
          format: email
        password:
          type: string
          format: password
          example: qwertyuiop
      required:
        - email
        - password
    Credentials:
      type: object
      properties:
        email:
          type: string
          format: email
        password:
          type: string
          format: password
          example: qwertyuiop
      required:
        - email
        - password
    Session:
      type: object
      properties:
        token:
          type: string
          example: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
    Cart:
      type: object
      properties:
        id:
          type: integer
        total:
          type: number
          format: decimal
        products:
          type: array
          items:
            $ref: '#/components/schemas/Product'
      required:
        - products
    CartRequest:
      type: object
      properties:
        products:
          type: array
          items:
            $ref: '#/components/schemas/ProductRequest'
    Product:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        price:
          type: number
        description:
          type: string
      required:
        - name
        - price
    ProductRequest:
      type: object
      properties:
        name:
          type: string
        description:
          type: string
        price:
          type: number
          format: decimal
        stock:
          type: integer
    OrderRequest:
      type: object
      properties:
        payment_method:
          type: string
          example: CASH
      required:
        - payment_method
    Order:
      type: object
      properties:
        id:
          type: integer
        products:
          type: array
          items:
            $ref: '#/components/schemas/Product'
      required:
        - products
    ReviewRequest:
      type: object
      properties:
        product_id:
          type: integer
        score:
          type: number
        comment:
          type: string
      required:
        - product_id
        - score
    Review:
      type: object
      properties:
        score:
          type: number
        comment:
          type: string
    Error:
      type: object
      properties:
        code:
          type: integer
          example: "000-154"
        message:
          type: string
          example: "Mensaje de error"
```

