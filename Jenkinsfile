def gitUrl = "git@GH510S:510S/TestGitHub2.git"
def targetBranch = "master"
def ChocolateJenkins = "git"
pipeline { // Declarative pipelineであることを宣言する
  agent any // 環境の指定（anyなので指定なし）
  tools {
    maven "M3"
  }
  // Mavenのビルドステップ
  // https://korattablog.com/2020/10/08/maven-architecture/
  stages{
    stage("checkout git") {
      steps{
        echo "Start checkout git!"
        dir("${WORKSPACE}") {
          git url: "${gitUrl}", branch: "${targetBranch}", credentialsId: "${ChocolateJenkins}"
          sh "ls -la"
        }
      }
    }

	/*
    stage("Lint") {
      steps{
        dir("${WORKSPACE}") {
          println("TODO: Add lint")
        }
      }
    }
    */

    stage("Unit Test") {
      steps{
        echo "Start Unit Test!"
        dir("${WORKSPACE}") {
          sh "mvn test"
        }
      }
    }

    stage("build") {
      steps{
        echo "Start build!"
        dir("${WORKSPACE}") {
          sh "mvn install"
        }
      }
      post{
        //常に実行
        always{
          echo "========End build!========"
        }
        //成功時
        success{
          echo "========Success!!========"
        }
        //失敗時
        failure{
          echo "========Fail……========"
        }
      }

    }

	/*
    stage("Integration Test") {
      steps{
        dir("${WORKSPACE}") {
          println("TODO: integration test")
        }
      }
    }

    stage("deploy") {
      steps{
        println("deploy delicious chocolate!!")
      }
    }
    */
  }
  post{
    always{
      // 成果物を保存
      archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
      cleanWs() // ワークスペース削除？
      echo "========Finish========"
    }
  }
}
