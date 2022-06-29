#!groovy

node {
	properties([
    	pipelineTriggers([
    		issueCommentTrigger('([\\s\\S]*)'),
        	pullRequestTrigger(pullRequestStates: ['opened', 'edited', 'reopened'])
    	])
	])

	println "excute"

	if (env.GITHUB_COMMENT != null) {
		println "issueCommentTrigger trigge"
	} else {
		println "pullRequestTrigger trigge"
	}
}