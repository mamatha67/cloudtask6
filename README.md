# cloudtask6
resource "kubernetes_deployment" "wordpress" {
metadata {
name = "wordpress1"
labels = {
test = "MyExampleApp"
}
}
spec {
replicas = 1
strategy {
type = "RollingUpdate"
}
selector {
match_labels = {
type = "cms"
env = "prod"
}
}
template {
metadata {
labels ={
type = "cms"
env = "prod"
}
}
spec {
container {
image = "wordpress"
name  = "wordpress1"
port{
container_port = 80
}
}
}
}
}
}
resource "kubernetes_service" "Nodeport" {
depends_on=[kubernetes_deployment.wordpress]
metadata {
name = "terraform-example"
}
spec {
selector = {
         prod = "wordpress"
         data = "mysql"
    }
    
    port {
      port        = 80
      
    }

    type = "NodePort"
}
}
