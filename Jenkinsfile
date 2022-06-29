#!groovy

node {
	properties([
    	pipelineTriggers([
    		issueCommentTrigger('([\\s\\S]*)'),
    		pullRequestReview(reviewStates: ['approved']),
        	pullRequestTrigger()
    	])
	])

	println "excute"
}