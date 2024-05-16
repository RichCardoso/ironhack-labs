# Prerequisitos
 - Git
 - iTerm
 - Oh my Zsh (https://kapeli.com/cheat_sheets/Oh-My-Zsh_Git.docset/Contents/Resources/Documents/index)

# 1. Branch Strategy Simulation

## Trunk Based Development

Se inicializa el repositorio y se crea el commit inicial en la rama **main** creando un archivo `HelloWorld.java`
```
git init
git remote add origin git@github.com:RichCardoso/lab-git-workflows.git
nano HelloWorld.java
gaa HelloWorld.java
gcmsg "initial commit"
git push --set-upstream origin main
```

Resultado:
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/12fb7f0b-9c93-48a8-ae4f-bd9b2b293d7b)

## GitFlow

Se crea una rama **develop**
```
gco -b develop
git push --set-upstream origin develop
```
Resultado:
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/b8e74a7e-3607-4164-b20b-39bfc03fa78e)

Se crea una rama **feature/comments** donde se agregaran nuevos cambios
```
gco -b feature/comments
git push --set-upstream origin feature/comments
```
Resultado:
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/85c9686c-ddab-4afa-89b2-7dbf4de5a709)


Se agrega un nuevo cambio en el archivo `HelloWorld.java` de la rama **feature/comments**
```
nano HelloWorld.java
gaa HelloWorld.java
gcmsg "se modifican comentarios de la clase main"
gp
```
Resultado:
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/b804f665-ad76-4362-92aa-ced64bf6d59d)

Se realiza merge desde la terminal, de la rama **feature/comments** a la rama **develop**
```
gco develop
gm feature/comments
gp
```
Resultado:
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/c12aad24-7c03-4697-a6fc-8c3c91eaed49)

# 2. Conflict Resolution and Merging

## Generate conflicts

Se genera una nueva rama **feature/print-adittional-info** en la que se modifica la impresion en pantalla
```
nano HelloWorld.java
gaa .
gcmsg "se agrega fecha actual al imprimir"
gp
```
Resultado:
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/6cfa414f-ae7d-4ce3-b1ee-efee2492a5b4)

Se modifica directamente en la rama **develop**
```
gco develop
nano HelloWorld.java
gaa .
gcmsg "se modifica funcion imprimir con nombre de autor"
gp
```
Resultado:
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/10bfe991-9a88-4bd7-8f0e-817e9db42328)

Se procede a realizar el merge de la rama **feature/print-adittional-info** a la rama **develop** y se obtiene un conflicto
```
gco develop
gm feature/print-adittional-info
```
Resultado:
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/731a6bb0-4860-40c2-b0d4-2b2f817f7002)

## Resolve conflicts

Se procede a resolver el conflicto dejando la linea del commit entrante de la rama **feature/print-adittional-info**
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/33c10548-0019-402c-be92-e2ff06775c79)

Se commitea la resolucion del conflicto y se sube al repositorio
```
gaa .
git commit
gp
```
Resultado:
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/c7cdef15-4b96-4d67-9c47-3ed0826571cd)

# 3. Pull Request and Code Review Simulation

## Create a pull request

Se genera una nueva clase para agregar al repo en una nueva rama **feature/add_model**
```
gco -b feature/add_model
nano Person.java
```
Resultado:
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/837cd0ac-5e6f-4508-b6fd-90a8c23a5380)

Se agrega la clase creada al repositorio y se sube el cambio
```
gaa Person.java
gcmsg "se agrega objeto de persona"
git push --set-upstream origin feature/add_model
```

Se genera el pull request en github de la rama **feature/add_model** a la rama **develop**
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/078b7a54-d098-43f6-a2dc-34ae6a29a010)

Se aprueban cambios del PR por medio de un reviewer
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/9ed8faec-8060-4a3c-8eeb-4960cf6b0d70)

Se procede a relizar merge
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/c4361460-3b41-4085-b126-dd23ac5ed7c9)

## Review another participant’s pull request

Se contribuye revisando y apoyando los PR's de otros participantes
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/989f90ad-cf12-480f-aa87-29b5f4fc2a17)

# 4. Automating with GitHub Actions

## Write a simple GitHub Actions workflow file

Se crea el directorio de `.github/workflows` y dentro se crea un archivo de tipo `.yml` donde estara el **github-action**
```
mkdir .github
cd .github
mkdir workflows
cd workflows
nano run-main.yml
```

Se agregan los campos necesarios para crear un action que ejecute el archivo `HelloWorld.java`
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/fcc56da1-efad-4a22-85e5-edafb203e11f)

Se sube el cambio en la rama develop que es donde esperamos se ejecute al crear un pull_request
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/ba560aef-f215-428b-b970-966bdcf0c975)

## Run the workflow

Se genera un pull request a la ramam de **develop** y se comprueba la ejecucion de **github-action**
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/30924d4b-7aa5-4c1c-a0f9-75fc5e708c3d)

Se visualiza que los steps hayan corrido exitosamente en el pull request
> ![image](https://github.com/RichCardoso/lab-git-workflows/assets/129906460/15f23768-69cf-491a-aded-1adb537d491f)





