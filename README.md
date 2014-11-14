Para poder contribuir a esta documentación hay que seguir los siguientes pasos:

* Crear una carpeta comun para alojar los branches master y gh-pages.
 
* Desde esta carpeta ejecutar los siguiente:

        git clone https://github.com/irontec/klear-tutorial.git --branch gh-pages --single-branch _build/html
        git clone https://github.com/irontec/klear-tutorial.git --branch master --single-branch source

Una vez hechos cambios en la carpeta source hacer lo siguiente desde esa carpeta:

    make clean
    make html

Después de esto hacer los commits y los push correspondientes en la carpeta source y en la carpeta _build/html.
