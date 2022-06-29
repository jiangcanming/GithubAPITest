#!groovy

node {
	properties([
    	pipelineTriggers([
    		issueCommentTrigger('([\\s\\S]*)'),
        	pullRequestTrigger(pullRequestStates: ['open', 'edited', 'reopened'])
    	])
	])

	def triggerCause = currentBuild.rawBuild.getCause(org.jenkinsci.plugins.pipeline.github.trigger.pullRequestCause)

	if (triggerCause) {
	    echo("Build was started by pullRequestCause")
	} else {
	    echo('Build was not started by a pullRequestCause')
	}

	println "excute"
}