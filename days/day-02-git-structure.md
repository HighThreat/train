# Estructura de carpetas

En este día simplemente voy a generar las carpetas que utilizaré posteriormente en otros días del proyecto, para ello vamos a utilizar comandos de git, dentro de git bash.
Ahora verás los comandos que realicé y después explicaré su función.


```bash
$ git init                                                 # Iniciamos git, si no inicia no podemos trabajar con los comandos.
Initialized empty Git repository in (censurado)
$ git remote add origin hhttps:URL/repositorio             # Fallo, la doble h en la url no nos permitirá realizar el siguiente tras branch -M main comando
$ git branch -M main
$ git remote -v
origin  https://github.com/HighThreat/train.git (fetch)
origin  https://github.com/HighThreat/train.git (push)
$ git remote remove origin                                 # Quitamos de git nuestra url del repositorio ya que hemos producido el fallo adrede.
$ git remote set-url origin https://github.com/HighThreat/train.git               
git remote -v                                              # Comprobamos de nuevo, tras ello realizamos los comandos para bajarnos el repositorio desde el main.
origin  https://github.com/HighThreat/train.git (fetch)
origin  https://github.com/HighThreat/train.git (push)
$ git pull origin main
                                                           # Creamos las carpetas, comandos de bash.
$ pwd                     
$ ls
$ mkdir days docs notes screenshots scripts
$ touch days/day-02-git-structure.md
$ touch notes/day-02-notes.md
$ git status
                                                           # Subimos los cambios al main para que se efectuen.
$ git add .
$ git commit -m "Día 2: estructura de carpetas y base del repo"
$ git push origin main

```

Con esto termina el día 2.
