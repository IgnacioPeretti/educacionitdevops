// Definimos LOGIN y PASSWORD 

def LOGIN
def PASSWORD

// Comenzamos el pipeline y definimos los parametros que vamos a utiliza.
pipeline {
    agent any                
    parameters {
        string(name: 'name', defaultValue: '', description: 'Nombre')
        string(name: 'surname', defaultValue: '', description: 'Apellido')
        choice(name: 'departament', choices: ['contabilidad', 'finanzas', 'tecnologia'], description: 'Selecciona el departamento')
    }  
// Primer stage creamos el usuario y le asignamos un departamento.
    stages {
        stage("Creación de usuario y asignamiento de grupo") {
            steps {
                script {
                    //Se concatena el nombre y apellido para formar el login
                    LOGIN = "${params.name}${params.surname}"
                }
                  //Creación del usuario
                sh "sudo useradd -m -c '${params.name} ${params.surname}, Departamento de ${params.departament}' ${LOGIN}"

   //Se asigna el usuario a un grupo (departamento)
                sh "sudo usermod -aG ${params.departament} ${LOGIN}"
            }
        }
      // Generamos una contraseña aleatoria-temporal.
        stage("Generación de password") {
            steps {
                script {
                    //Se genera una password aleatoria
                    PASSWORD = sh(script: 'openssl rand -base64 12', returnStdout: true).trim()
                }
                //Se asigna la passsword aleatoria-temporal al usuario
                sh "echo '${LOGIN}:${PASSWORD}' | sudo chpasswd"
                sh "sudo passwd -e ${LOGIN}"
            }
        }
        // El operador obtiene la contraseña para enviársela al usuario.
        stage("Visualización de password temporal") {
            steps {
                //Se muestra la contraseña que será enviada al usuario
                echo "La contraseña del usuario ${LOGIN} es ${PASSWORD}"
            }
        }
    }
}
