#!groovy

node {
	properties([
    	pipelineTriggers([
        	issueCommentTrigger('([\\s\\S]*)')
    	])
	])

	if (env.GITHUB_COMMENT == null) {
		return
	}

	println env.GITHUB_COMMENT
}