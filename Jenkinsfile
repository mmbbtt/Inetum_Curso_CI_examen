diaActual = new Date().getDay()

pipeline 
{
    agent any
    
    environment
    {
        WS_EXAMAEN = "D:\\AT\\pers\\CursoCI\\ws_examen"
        PATH_DEPLOY = "D:\\AT\\pers\\CursoCI\\ws_examen\\deploy\\"
        URL_PROYECTO_GITHUB = "https://github.com/mmbbtt/Inetum_Curso_CI_examen"
    }
    
    options
    {
        timeout(time: 5, unit: 'MINUTES' )
    }
    
    tools
    {
       maven 'Maven'  
    }

    stages 
    {
        stage('Operaciones aritméticas') 
        {
            when
            {
                equals(actual: diaActual, expected: 4)
            }
            
            steps 
            {
               script
               {
                   numero1 = 100
                   numero2 = 50
                   
                   suma = numero1 + numero2
                   resta = numero1 - numero2
                   multiplicacion = numero1 * numero2
                   if(numero2 == 0)
                   {
                       division = 'No se puede dividir por cero'
                   }
                   else
                   {
                       division = numero1 / numero2
                   }
                   potencia1 = numero1 * numero1
                   potencia2 = numero2 * numero2
                   
                   resultado = "SUMA: ${numero1} + ${numero2} = ${suma}"
                   resultado = resultado + "\nRESTA: ${numero1} - ${numero2} = ${resta}"
                   resultado = resultado + "\nMULTIPLICACIÓN: ${numero1} x ${numero2} = ${multiplicacion}"
                   resultado = resultado + "\nDIVISIÓN: ${numero1} / ${numero2} = ${division}"
                   resultado = resultado + "\nPOTENCIA: ${numero1} = ${potencia1}"
                   resultado = resultado + "\nPOTENCIA: ${numero2} = ${potencia2}"
                   
                   archivoSalida = "operacionesAritmeticas.txt"
                   writeFile(file: archivoSalida, text: resultado)
               }
            }
        }
        
        stage('Clonado rama principal') 
        {
            when
            {
                equals(actual: diaActual, expected: 4)
            }
            
            steps 
            {
               git branch: "master", url: URL_PROYECTO_GITHUB
               echo "Rama master clonada"
            }
        }
        
        stage('Tareas Maven') 
        {
            when
            {
                equals(actual: diaActual, expected: 4)
            }
            
            steps 
            {
               bat "mvn test"
               bat "mvn clean install"
            }
        }
        
        stage('Deploy ') 
        {
            when
            {
                equals(actual: diaActual, expected: 4)
            }
            
            steps 
            {
               script
               {
                   def pathWorkspace = "${env.WORKSPACE}"
                   pathWorkspace = pathWorkspace.trim()
                   pathWorkspace = pathWorkspace.replace("\\", "\\\\")
                        
                   //Copiar el jar generado en la carpeta del deploy
                   files = findFiles(glob: "**/target/*.jar") 
                   if(files.size() > 0)
                    {

                        pathJars = pathWorkspace + "\\target\\*.jar"
                        
                        bat "cmd /c xcopy ${pathJars} ${PATH_DEPLOY} /y"
                        
                        files.each()
                        {
                            
                            println "Copiado el archivo ${it} a ${PATH_DEPLOY}"
                        }
                    }
                    else
                    {
                        println "ERROR: No se encuentra el archivo a desplegar"
                    }
                    
                    //Copiar el fichero con el resultado del día lunes
                    
                    bat "cmd /c xcopy ${archivoSalida} ${PATH_DEPLOY} /y"
                    
                    //Leer el archivo de operaciones y mostralo
                    contenido = readFile(file: archivoSalida)
                    println(contenido)
               }
            }
        }
        
        stage('Infomre') 
        {
            when
            {
                equals(actual: diaActual, expected: 4)
            }
            
            steps 
            {
               script
               {
                   println "Tarea ejecutada por ${env.USERNAME}"
               }
            }
        }
    }
}
