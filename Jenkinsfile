node  {
					stage 'SCM'
					bat 'echo SCM built successfully'
					stage 'Build'
					bat 'echo Build built successfully'
					stage 'Analysis'
					bat 'echo Build Analysis successfully'				
					stage 'CF push'
					bat 'echo CF push Analysis successfully'				
					stage 'Test'
					try {
		
									bat "echooooooooo test suite is completed"
					}catch(err) {
						try {
							timeout(time: 15, unit: 'SECONDS') {
								public def userInput = input(id: 'UserInput', message: 'Approval ', parameters: [[$class: 'TextParameterDefinition', defaultValue: 'Yes', description: 'Approval', name: 'Approval']])
								env.ENV = userInput
							}
						} catch (err1) {
							CF_app()
							return
						}
						if ( env.ENV  == "Yes") {
								CF_app()
								return
						} else {	
							currentBuild.result = 'FAILURE'
							return
						}
						
					} 
					CF_app()
}

def CF_app() {
	stage 'Map route'
	bat "echo Mapped the route"
	stage 'Unmap route'
	bat "echo Unmapped the route"
	stage 'Delete app'
	bat "echo Deleted the app"
	stage 'Promote'
	parallel (CloudFoundry_push_to_QA: {
		bat "echo Successfully pushed to QA env"
		build job: 'Deploy to QA'
		}, Cloud_Foundry_push_to_UAT: {
		bat "echo Successfully pushed to UAT env"
		build job: 'Deploy to UAT'
		}, Cloud_Foundry_push_to_INT: {
		bat "echo Successfully pushed to INT env"
		build job: 'Deploy to INT'
	})
}
