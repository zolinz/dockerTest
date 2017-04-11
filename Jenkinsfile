node {
  def project = 'git-repo-test-01'
  def appName = 'zoli-spring'
  def feSvcName = "${appName}-frontend"
  def imageTag = "gcr.io/${project}/${appName}"

  checkout scm

  stage 'Build image'
  sh("docker build -t zoli-spring .")
  sh("docker tag zoli-spring gcr.io/git-repo-test-01/zoli-spring")

  stage 'Setup prod cluster'
  sh("gcloud config set container/cluster kubernetes-cluster")
  sh("gcloud container clusters get-credentials kubernetes-cluster")

  stage 'Push image to registry'
  sh("gcloud docker push ${imageTag}")

  stage "Deploy Application"
  sh("kubectl run zoli-spring --image gcr.io/git-repo-test-01/zoli-spring")
  sh("kubectl expose deployment zoli-spring --port=80 --target-port=8080 --name=zoli-spring-service --type=LoadBalancer")

}
