#!groovy

node {
	properties([
    	pipelineTriggers([
    		issueCommentTrigger('([\\s\\S]*)'),
    		pullRequestReview(reviewStates: ['approved']),
        	pullRequestTrigger(pullRequestStates: ['open', 'edited', 'reopened'])
    	])
	])

	println "excute"
}