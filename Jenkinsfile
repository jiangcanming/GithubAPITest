#!groovy

node {
	properties([
    	pipelineTriggers([
    		issueCommentTrigger('([\\s\\S]*)'),
        	pullRequestTrigger()
    	])
	])

	println "excute"
}