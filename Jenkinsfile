pipeline {
    agent any
    node {
      checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'fortify']], gitTool: 'Default', submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'b8bc3a8d-5231-4f5c-8b67-dd04540c2680', url: 'https://github.cms.gov/FFMTools/ffm-fortify.git']]])
    }
    	    
    stages {
        stage('Fortify Scala-Translation') {
            steps {
	            sh "sourceanalyzer -Xmx10G -b 'dsrs-app' -Dcom.fortify.sca.limiters.MaxIndirectResolutionsForCall=200 -Dcom.fortify.sca.limiters.MaxSink=256 -Dcom.fortify.sca.limiters.MaxSource=256 -Dcom.fortify.sca.limiters.MaxNodesForGlobal=256 -logfile: '${WORKSPACE}/Fortify-log/scan.log' -scan -f '$BRANCH.fpr'"
	    }
	}
        stage('Fortify JAVA-Translate') {
            steps {
                    fortifyTranslate addJVMOptions: '', buildID: 'dsrs-app', excludeList: '"**/test/*:**/build/*:**/*.scala"', logFile: '', maxHeap: '', projectScanType: fortifyAdvanced(advOptions: '''-source 1.8
-cp $WORKSPACE/**/*.jar:/opt/cms/jdk8/**/*.jar $WORKSPACE/dsrs-app/**/*''')
           }
       }
        stage('Fortify Scan') {
            steps {
                    fortifyScan addJVMOptions: '', addOptions: '-Dcom.fortify.sca.limiters.MaxIndirectResolutionsForCall=200 -Dcom.fortify.sca.limiters.MaxSink=256 -Dcom.fortify.sca.limiters.MaxSource=256 -Dcom.fortify.sca.limiters.MaxNodesForGlobal=256 -append', buildID: 'dsrs-app', customRulepacks: '', debug: true, logFile: '${WORKSPACE}/Fortify-log/scan.log', maxHeap: '20000', resultsFile: '$BRANCH.fpr', verbose: true
            }
        }
			
        stage('Fortify Upload') {
            steps {
                    fortifyUpload appName: 'FFM-DSRS-Dev', appVersion: '$BRANCH', resultsFile: '$BRANCH.fpr'
            
            }
        }
    }
   
}
