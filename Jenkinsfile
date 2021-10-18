node {
    stage("NodeJS setup") {
        env.NODEJS_HOME="${tool 'nodejs'}"
        env.PATH="${env.NODEJS_HOME}/bin:${env.PATH}"
        sh "echo `npm --version`"
        sh "rm -rf *.tar.gz"
}
    stage("Checkout") {
        git url: "https://github.com/MariiaHerasymenko/material-design-template"
}
    stage("Compressing") {
            parallel (
                "Compress-js": {
                    sh "ls www/js/ | xargs -I{i} uglifyjs www/js/{i} -o www/min/{i} -c"

},
                "Compress-css": {
                    sh "ls www/css/ | xargs -I{i} cleancss www/css/{i} -o www/min/{i}"
        }
    )  
}
    stage("Archiving results") {
        sh "tar --exclude='www/js' --exclude='www/css' --exclude='.git' -zcvf results.tar.gz *"
        archiveArtifacts artifacts: 'results.tar.gz', onlyIfSuccessful: true
    }
    stage("Artifactory") {
	rtUpload (
	   serverId: "mariia-jfrog",
	   spec: """{
		"files": [
		    {
			"pattern": "results.tar.gz",
			"target": "generic-local"
		    }
		]
	   }"""
	)
     }
}
