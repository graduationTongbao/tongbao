node {//运行节点
    stage('SCM') {//运行阶段
    //从github上拉取代码
        git 'https://github.com/graduationTongbao/tongbao.git'
    }
    stage('build') {
    //用Maven来打包
        def mvnHome = tool 'M3'//之前配置的Maven工具
        sh "${mvnHome}/bin/mvn -B clean package"//maven的清理和打包
    }
    stage('deploy') {
    //用Docker部署，其中的‘my’可以自定义
         sh "docker stop my || true"//停止之前运行的容器
         sh "docker rm my || true"//删除之前运行的容器	
         sh "docker run -d -it --name my -p 22222:8080 -d tomcat"//启动容器，命名为‘my’，将宿主机的11111端口映射到容器的8080端口（11111可以修改，8080不能修改）
         sh "mv target/tongbao.war target/ROOT.war"
         sh "docker cp target/ROOT.war my:/usr/local/tomcat/webapps"
         sh "docker exec -d my rm -rf /usr/local/tomcat/webapps/ROOT"
    }
    stage('results') {
    //生成制品
        archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
    }
}
